+++
title = "Dùng Eclipse"
date = 2021
weight = 1
chapter = false
pre = "<b>Cách A. </b>"
+++

Trường hợp của chúng ta mô phỏng sử dụng mẫu dữ liệu bán hàng thực tế và được xử lý qua Kinesis stream và được ghi vào ElasticSearch nơi chúng ta có thể phân tích dữ liệu bán hàng bằng đồ thị với Kibana. Dữ liệu bán hàng sẽ được đọc từ tệp CSV và có chi phí, số lượng và ngày giao dịch được áp dụng ngẫu nhiên. Kiến trúc trông như thế này:

![SQS](/images/3/kinesisconsumerproducerapparch.png?width=50pc)

**Nội dung**
- [Tạo một kho lưu trữ mới cho mã nguồn Kinesis Consumer](#tạo-một-kho-lưu-trữ-mới-cho-mã-nguồn-kinesis-consumer)
- [Sao chép kho lưu trữ vào Kinesis Consumer EC2 instance](#sao-chép-kho-lưu-trữ-vào-kinesis-consumer-ec2-instance)
- [Tạo một kho lưu trữ mới cho mã nguồn Kinesis Producer](#tạo-một-kho-lưu-trữ-mới-cho-mã-nguồn-kinesis-producer)
- [Sao chép kho lưu trữ vào Kinesis Producer EC2 instance](#sao-chép-kho-lưu-trữ-vào-kinesis-producer-ec2-instance)

#### Tạo một kho lưu trữ mới cho mã nguồn Kinesis Consumer

1. Mở **AWS CodeCommit console** trong trình duyệt.
2. Ở trang dịch vụ, chọn **Get Started**
3. Ở trang **Create repository**, mục **Repository name** nhập ```KinesisConsumer```
4. Ở **Description** nhập ```Sample application that consumes messages from a Kinesis stream and writes them to an ElasticSearch domain index```.

![MessageStreaming](/images/3/1.png?width=90pc)

5. Chọn **Create repository**. Kho lưu trữ mới của bạn sẽ được tạo, sẵn sàng cho bạn sử dụng. Chọn vào **Skip** để đóng màn hình tiếp theo. Chọn vào **Close** để đóng bảng điều khiển xuất hiện sau khi tạo kho lưu trữ.

6. Trong **Eclipse IDE**, mở **Git Perspective**. Nếu bạn chưa có khung thông tin này, hãy sử dụng lựa chọn menu **Window | Perspective | Open Perspective | Git repositories**.
7. Chọn biểu tượng **Clone repository**

![MessageStreaming](/images/3/2.png?width=90pc)

8. Trong **AWS CodeCommit console** trong trình duyệt, nhấp vào nút **Clone URL**. Chọn **HTTPS** từ menu và sau đó sao chép URL vào khay nhớ tạm của bạn.

![MessageStreaming](/images/3/3.png?width=90pc)

9. Quay lại **Eclipse IDE**, dán URL bản sao vào trường **URI**:

![MessageStreaming](/images/3/4.png?width=90pc)

10. Trong mục **Authentication**, ở mục **User** nhập vào giá trị GitUsername từ thông tin ở tab **Cloudformation Output**.
11. Ở mục **Password**, nhập giá trị **GitPassword** từ thông tin ở tab **Cloudformation Output**.

![MessageStreaming](/images/3/5.png?width=90pc)

12. Chọn **Next**.
13. Trong **Branch Selection**, không có branch nào được tạo, vì vậy chỉ cần chọn vào **Next**.
14. Trong bảng **Local destination**, chọn một đường dẫn phù hợp trên filesystem của bạn và chọn vào **Finish:**.

![MessageStreaming](/images/3/6.png?width=90pc)

Kho lưu trữ trống sẽ được sao chép vào đường dẫn local của bạn đã chỉ định.

15. Tải xuống gói mã nguồn từ [**KinesisConsumer.zip**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/KinesisConsumer.zip) và giải nén tập tin zip vào hệ thống của bạn.
16. Sao chép nội dung của tập tin zip đã giải nén sang vị trí nơi bạn đã sao chép kho **KinesisConsumer** về.
17. Trong **Eclipse IDE**, chuyển sang môi trường Java và mở Project với lựa chọn **File | Open projects from filesystem...**.
18. Trong **Eclipse IDE**, mở tập tin **config.java**.

![MessageStreaming](/images/3/7.png?width=90pc)

19. Cập nhật giá trị `public static final String ELASTICSEARCH_ENDPOINT` trung với thông tin từ tab **Cloudformation Output**, gọi đến **ElasticsearchEndpoint**. Mặc định, giá trị sẽ được thiết lập ở vị trí ```REPLACE_WITH_YOUR_ELASTIC_SEARCH_ENDPOINT```. Xóa và thay thế với giá trị **ElasticsearchEndpoint** của bạn. Lưu lại tập tin.
20. Mở cửa sổ lệnh và chạy lệnh ```cd``` vào thư mục tải về từ kho dữ liệu của bạn.
21. Tạo một branch master mới bằng lệnh sau:

```bash
git checkout -b master
```

22. Thêm các tập tin mới vào kho lưu trữ bằng lệnh:

```bash
git add .
```

23. Xác nhận các tập tin mới vào branch bằng lệnh sau:

```bash
git commit -m "Initial commit"
```

24. Trong khung quản lý Git ở **Eclipse IDE**, chuột phải vào kho dữ liệu *KinesisConsumer*, và chọn **Push Branch Master**

{{% notice note %}}
Nếu tùy chọn này không xuất hiện hoặc không có branch **Master**, bạn có thể cần làm mới để cập nhật IDE.
{{% /notice %}}

25. Chọn **Finish**. Branch sẽ được đẩy lên kho dữ liệu CodeCommit. Xác nhận bằng việc làm mới lại trang **CodeCommit console** trên trình duyệt:

![MessageStreaming](/images/3/8.png?width=90pc)

Bạn sẽ thấy các tập tin mà bạn đã thêm vào kho lưu trữ được liệt kê trong giao diện trình duyệt.

#### Sao chép kho lưu trữ vào Kinesis Consumer EC2 instance

Trong thiết lập bài thực hành đã cung cấp một EC2 instance để bạn chạy với tư cách Consumer trong tình huống này. Bây giờ bạn sẽ kết nối với EC2 instance, sao chép kho lưu trữ về, build và chạy chương trình Consumer.

26. Để kết nối an toàn với cơ sở hạ tầng đám mây của bạn, bạn sẽ sử dụng SSH. SSH yêu cầu một cặp public/private key-pair được thiết lập và cài đặt trên máy client và server sẽ giao tiếp thông qua giao thức SSH. Các EC2 instance đã được khởi chạy với một public key. Bạn cần truy xuất private key để có thể sử dụng khóa đó để thiết lập kết nối an toàn. 

{{% notice note %}}
Hãy đảm bảo bạn đã có key-pair được thiết lập khi triển khai CloudFormation. Lưu tập tin khóa này trên máy của bạn.
{{% /notice %}}

27. Trong một cửa sổ lệnh trên máy của bạn, hãy chạy lệnh sau. Lệnh này sẽ đảm bảo bạn có thể sử dụng keypair đã tải xuống để đăng nhập vào các EC2 instance mà bạn sẽ sử dụng.

```bash
chmod 400 <path to your downloaded PEM file>
```

**Ví dụ:**
```bash
chmod 400 ~/Downloads/ee-default-keypair.pem
```

{{% notice note %}}
Để mở cửa sổ lệnh, bạn sẽ quay lại chúng một chút nữa.
{{% /notice %}}

28. Trong **AWS Console**, trang **CloudFormation**, trong tab **Outputs**, tìm giá trị **EC2InstanceKinesisConsumer** và sao chép vào bộ nhớ tạm.

![MessageStreaming](/images/3/9.png?width=90pc)

29. Sử dụng lệnh mẫu sau để kết nối đến EC2 instance của bạn:

```bash
ssh -i <path to your downloaded PEM file> ec2-user@<EC2InstanceKinesisConsumer>
```

**Ví dụ:**
```bash
ssh -i ~/Downloads/ee-default-keypair.pem ec2-user@34.212.21.7
```

{{% notice warning %}}
Đừng thêm vào **<** và **>**.Nếu bạn có khó khăn ở bước này, hãy tìm hướng dẫn. Khi đã hoàn thành việc đăng nhập vào instance, tiếp tục các bước bên dưới.
{{% /notice %}}


30. Ở cửa sổ lệnh bạn SSH vào instance, sử dụng lệnh sau để tải về từ kho dữ liệu của bạn:
```bash
git clone https://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/KinesisConsumer
```
Bạn sẽ bị hỏi username và password. Bạn có thể lấy thông tin này từ tab **Cloudformation Outputs** - tìm thông tin **GitUsername** và **GitPassword**.

Bạn sẽ nhận được phản hồi sau
```bash
Cloning into 'KinesisConsumer'...
remote: Counting objects: 18, done.
Unpacking objects: 100% (18/18), done.
```

31. Mã nguồn bạn đã tải lên từ máy trạm của mình bây giờ sẽ được sao chép vào EC2 instance và sẽ mất một chút thời gian để hoàn thành.
32. Máy chủ đã được cài đặt Maven nên bạn có thể build mã nguồn ứng dụng. `cd` vào thư mục `KinesisConsumer` và thực thi lệnh:
```bash
cd KinesisConsumer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
Sẽ mất một chút thời gian để build mã nguồn ứng dụng.

33. Khi quá trình build hoàn tất, chạy ứng dụng Kinesis Consumer bằng lệnh:
```bash
java -jar target/KinesisConsumerApp-1.0.0.jar
```

Ứng dụng sẽ khởi động và chờ các sự kiện được gửi vào Kinesis stream. Bạn sẽ không thấy lỗi nào và sau một lúc, đầu ra của bảng điều khiển sẽ hiển thị `Sleeping...` vì không có bản ghi nào trong luồng để xử lý:
```text
INFO: Initializing record processor for shard: shardId-000000000000
Aug 21, 2017 11:30:19 AM idevelop.demo.RecordProcessor initialize
INFO: Initializing record processor for shard: shardId-000000000001
Aug 21, 2017 11:30:57 AM com.amazonaws.services.kinesis.clientlibrary.lib.worker.Worker info
INFO: Current stream shard assignments: shardId-000000000001, shardId-000000000000
Aug 21, 2017 11:30:57 AM com.amazonaws.services.kinesis.clientlibrary.lib.worker.Worker info
INFO: Sleeping ...
```
Dừng việc thực thi trong cửa sổ lệnh bằng tổ hợp phím `CTRL + C`

#### Tạo một kho lưu trữ mới cho mã nguồn Kinesis Producer 

Bây giờ bạn sẽ lặp lại quá trình trên, lần này, tạo một kho lưu trữ cho ứng dụng **Kinesis Producer** và sao chép chúng vào Kinesis Producer EC2 instance. Sau khi hoàn tất, bạn có thể bắt đầu thử nghiệm truyền dữ liệu giữa hai máy thông qua Amazon Kinesis.

Quá trình tạo kho lưu trữ giống như các bước trước, nhưng chúng ta sẽ lặp lại nó ở đây để tiện theo dõi.

34. Mở **AWS CodeCommit console** in the browser.
35. Trong trang **Create repository**, ở **Repository name** nhập ```KinesisProducer```
36. Ở **Description** nhập ```Sample application that publishes sales events into a Kinesis stream```.
37. Chọn **Create repository**. Kho lưu trữ mới của bạn sẽ được tạo, sẵn sàng cho bạn sử dụng. Chọn vào **Skip** để đóng màn hình tiếp theo. Chọn vào **Close** để đóng bảng điều khiển xuất hiện sau khi tạo kho lưu trữ.

#### Sao chép kho lưu trữ vào Kinesis Producer EC2 instance
38. Trong **Eclipse IDE**, mở **Git Perspective**. Nếu bạn chưa có khung thông tin này, hãy sử dụng lựa chọn menu **Window | Perspective | Open Perspective | Git repositories**.
39. Chọn biểu tượng **Clone repository**
40. Trong **AWS CodeCommit console** trong trình duyệt, nhấp vào nút **Clone URL**. Chọn **HTTPS** từ menu và sau đó sao chép URL vào khay nhớ tạm của bạn.
41. Quay lại **Eclipse IDE**, dán URL bản sao vào trường **URI**:
42. Trong mục **Authentication**, ở mục **User** nhập vào giá trị GitUsername từ thông tin ở tab **Cloudformation Output**. Ở mục **Password**, nhập giá trị **GitPassword** từ thông tin ở tab **Cloudformation Output**.
43. Chọn **Next**.
44. Trong **Branch Selection**, không có branch nào được tạo, vì vậy chỉ cần chọn vào **Next**.
45. Trong bảng **Local destination**, chọn một đường dẫn phù hợp trên filesystem của bạn và chọn vào **Finish:**.
Kho lưu trữ trống sẽ được sao chép vào đường dẫn local của bạn đã chỉ định.

46. Tải xuống gói mã nguồn từ [**KinesisProducer.zip**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/KinesisProducer.zip) và giải nén tập tin zip vào hệ thống của bạn.
47. Sao chép nội dung của tập tin zip đã giải nén sang vị trí nơi bạn đã sao chép kho **KinesisProducer** về.
48. Mở cửa sổ lệnh và chạy lệnh ```cd``` vào thư mục tải về từ kho dữ liệu của bạn.
49. Tạo một branch master mới bằng lệnh sau:

```bash
git checkout -b master
```

50. Thêm các tập tin mới vào kho lưu trữ bằng lệnh:

```bash
git add .
```

51. Xác nhận các tập tin mới vào branch bằng lệnh sau:

```bash
git commit -m "Initial commit"
```

52. Trong khung quản lý Git ở **Eclipse IDE**, chuột phải vào kho dữ liệu *KinesisProducer*, và chọn **Push Branch Master**

![MessageStreaming](/images/3/10a.png?width=90pc)

53. Chọn **Next** và ở trang kế, chọn **Finish**.

Branch sẽ được đẩy lên kho dữ liệu CodeCommit. Xác nhận bằng việc làm mới lại trang **CodeCommit console** trên trình duyệt. Bạn sẽ thấy các tập tin mà bạn đã thêm vào kho lưu trữ được liệt kê trong giao diện trình duyệt.

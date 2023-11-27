+++
title = "Các bước chuẩn bị"
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++
#### Các bước chuẩn bị 

Trường hợp của chúng ta mô phỏng sử dụng mẫu dữ liệu bán hàng thực tế và được xử lý qua Kinesis stream và được ghi vào ElasticSearch nơi chúng ta có thể phân tích dữ liệu bán hàng bằng đồ thị với Kibana. Dữ liệu bán hàng sẽ được đọc từ tệp CSV và có chi phí, số lượng và ngày giao dịch được áp dụng ngẫu nhiên. Kiến trúc trông như thế này:
![Prequistite](/images/4-message-streaming/4.1-prequistite/kinesisconsumerproducerapparch.png?featherlight=false&width=70pc)

#### Tạo một kho lưu trữ mới cho mã nguồn Kinesis Consumer

1. Truy cập [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Click **Create repository**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-001.png?featherlight=false&width=90pc)
2. Trong trang **Create repository**
* Tại mục **Repository name**, nhập ```KinesisConsumer```
* Tại mục **Description**, nhập ```Sample application that consumes messages from a Kinesis stream and writes them to an ElasticSearch domain index```
* Click **Create**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-002.png?featherlight=false&width=90pc)
3. Trong trang **KinesisConsumer**, click **Clone URL**
* Click **Clone HTTPS** để lưu lại url
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-006.png?featherlight=false&width=90pc)
4. Trong **Eclipse IDE**, click **Window**
* Click **Perspective**
* Click **Open Perspective**
* Click **Other**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-003.png?featherlight=false&width=90pc)
5. Trong phần **Open Perspective**, click **Git**
* Click **Open**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-004.png?featherlight=false&width=90pc)
6. Click **Clone a Git repository**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-005.png?featherlight=false&width=90pc)
7. Trong phần **Source Git Repository**
* Tại mục **URI**, nhập url đã lưu trong bước 3
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-007.png?featherlight=false&width=90pc)
8. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Nhập ```DevAx-05``` vào ô tìm kiếm, nhấn **Enter**
* Click **DevAx-05**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-008.png?featherlight=false&width=90pc)
9. Click tab **Output**
* Lưu lại thông tin **GitUserName** và **GitPassword**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-009.png?featherlight=false&width=90pc)
10. Trong **Eclipse IDE**, điền thông tin **User** và **Password** đã lưu trong bước 9
* Click **Next**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-010.png?featherlight=false&width=90pc)
11. Click **Next**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-011.png?featherlight=false&width=90pc)
12. Click **Finish**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-012.png?featherlight=false&width=90pc)

{{%attachments title="KinesisConsumer Project" style="orange" pattern="KinesisConsumer.zip"/%}}

13. Tải file **KinesisConsumer.zip** và giải nén
* Sao chép nội dung của tập tin zip đã giải nén sang vị trí nơi bạn đã sao chép kho **KinesisConsumer** về.
14. Trong **Eclipse IDE**, Click **File**
* Click **Open projects from filesystem…**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-013.png?featherlight=false&width=90pc)
15. Chọn đường dẫn đến project **KinesisConsumer**
* Click **Finish**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-014.png?featherlight=false&width=90pc)
16. Mở file **KinesisConsumer\src\main\java\idevelop\demo\config.java**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-015.png?featherlight=false&width=90pc)
17. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Nhập ```DevAx-05``` vào ô tìm kiếm, nhấn **Enter**
* Click **DevAx-05**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-016.png?featherlight=false&width=90pc)
18. Click tab **Output**
* Lưu lại thông tin **ElasticsearchEndpoint**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-017.png?featherlight=false&width=90pc)
19. Trong **Eclipse IDE**, thay thế **REPLACE_WITH_YOUR_ELASTIC_SEARCH_ENDPOINT** bằng **ElasticsearchEndpoint** đã lưu ở bước 18
* Tại mục **REGION**, thay **ap-southeast-2** bằng **Region code** của bạn
* Lưu lại
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-018.png?featherlight=false&width=90pc)
20. Mở **Command Prompt** và chuyển đường dẫn đến project **KinesisConsumer**
* Chạy lệnh dưới đây để tạo một branch **master** mới
```
git checkout -b master
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-019.png?featherlight=false&width=60pc)
21. Chạy lệnh dưới đây để thêm các tập tin mới vào kho lưu trữ
```
git add .
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-020.png?featherlight=false&width=60pc)
22. Chạy lệnh dưới đây để commit tập tin mới
```
git commit -m "Initial commit"
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-021.png?featherlight=false&width=60pc)
23. Trong **Eclipse IDE**, chuột phải vào kho dữ liệu **KinesisConsumer**
* Click **Push Branch \`master\`...**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-022.png?featherlight=false&width=90pc)
24. Nhập thông tin **User** và **Password** đã lưu trong bước 9 vào mục **User** và **Password**
* Click **Login**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-023.png?featherlight=false&width=90pc)
25. Click **Preview**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-024.png?featherlight=false&width=90pc)
26. Click **Push**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-025.png?featherlight=false&width=90pc)
27. Truy cập [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Nhập ```KinesisConsumer``` vào ô tìm kiếm
* Click **KinesisConsumer**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-026.png?featherlight=false&width=90pc)
28. Bạn sẽ thấy các tập tin mà bạn đã thêm vào kho lưu trữ được liệt kê trong giao diện trình duyệt.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-027.png?featherlight=false&width=90pc)

#### Sao chép kho lưu trữ vào Kinesis Consumer EC2 instance

Trong thiết lập bài thực hành đã cung cấp một EC2 instance để bạn chạy với tư cách Consumer trong tình huống này. Bây giờ bạn sẽ kết nối với EC2 instance, sao chép kho lưu trữ về, build và chạy chương trình Consumer.

{{% notice note %}} 
Để kết nối an toàn với cơ sở hạ tầng đám mây của bạn, bạn sẽ sử dụng SSH. SSH yêu cầu một cặp public/private key-pair được thiết lập và cài đặt trên máy client và server sẽ giao tiếp thông qua giao thức SSH. Các EC2 instance đã được khởi chạy với một public key. Bạn cần truy xuất private key để có thể sử dụng khóa đó để thiết lập kết nối an toàn.\
Hãy đảm bảo bạn đã có key-pair được thiết lập khi triển khai CloudFormation. Lưu tập tin khóa này trên máy của bạn.
{{% /notice %}}

29. Mở **Command Prompt** và chạy lệnh dưới đây
```
chmod 400 <path to your key-pair file>
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-028.png?featherlight=false&width=60pc)
30. Chạy lệnh dưới đây
```
ssh -i <path to your key-pair file> ec2-user@<EC2InstanceKinesisConsumer>
```
{{% notice note %}} 
Thay **\<EC2InstanceKinesisConsumer\>** bằng giá trị **EC2InstanceKinesisConsumer** trong tab **Output** của stack **DevAx-05**
{{% /notice %}}
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-029.png?featherlight=false&width=60pc)
31. Chạy lệnh dưới đây
```
git clone https://git-codecommit.<your_region>.amazonaws.com/v1/repos/KinesisConsumer
```
{{% notice note %}} 
Thay **<your_region>** bằng **Region** của bạn
{{% /notice %}}
* Nhập **User** và **Password** đã lưu trong bước 9 vào mục **Username** và **Password**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-030.png?featherlight=false&width=60pc)
32. Chạy lệnh dưới đây
```
cd KinesisConsumer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-033.png?featherlight=false&width=60pc)
33. Sẽ mất một chút thời gian để build mã nguồn ứng dụng.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-034.png?featherlight=false&width=60pc)
34. Khi quá trình build hoàn tất, chạy ứng dụng Kinesis Consumer bằng lệnh:
```
java -jar target/KinesisConsumerApp-1.0.0.jar
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-035.png?featherlight=false&width=60pc)
Ứng dụng sẽ khởi động và chờ các sự kiện được gửi vào Kinesis stream. Bạn sẽ không thấy lỗi nào và sau một lúc, đầu ra của bảng điều khiển sẽ hiển thị **Sleeping...** vì không có bản ghi nào trong luồng để xử lý:
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-036.png?featherlight=false&width=60pc)

#### Tạo một kho lưu trữ mới cho mã nguồn Kinesis Producer
Bây giờ bạn sẽ lặp lại quá trình trên, lần này, tạo một kho lưu trữ cho ứng dụng Kinesis Producer và sao chép chúng vào Kinesis Producer EC2 instance. Sau khi hoàn tất, bạn có thể bắt đầu thử nghiệm truyền dữ liệu giữa hai máy thông qua Amazon Kinesis.

Quá trình tạo kho lưu trữ giống như các bước trước, nhưng chúng ta sẽ lặp lại nó ở đây để tiện theo dõi.

35.  Truy cập [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Click **Create repository**
36. Trong trang **Create repository**
* Tại mục **Repository name**, nhập ```KinesisProducer```
* Tại mục **Description**, nhập ```Sample application that publishes sales events into a Kinesis stream```
* Click **Create**
37. Trong trang **KinesisProducer**, click **Clone URL**
* Click **Clone HTTPS** để lưu lại url
38. Trong **Eclipse IDE**, click **Clone a Git Repository and add the clone to this view** icon
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037.png?featherlight=false&width=90pc)
39. Trong phần **Source Git Repository**
* Tại mục **URI**, nhập url đã lưu trong bước 37
40. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Nhập ```DevAx-05``` vào ô tìm kiếm, nhấn **Enter**
* Click **DevAx-05**
41. Click tab **Output**
* Lưu lại thông tin **GitUserName** và **GitPassword**
42. Trong **Eclipse IDE**, điền thông tin **User** và **Password** đã lưu trong bước 9
* Click **Next**
43. Click **Next**
44. Click **Finish**

{{%attachments title="KinesisProducer Project" style="orange" pattern="KinesisProducer.zip"/%}}

45. Tải file **KinesisProducer.zip** và giải nén
* Sao chép nội dung của tập tin zip đã giải nén sang vị trí nơi bạn đã sao chép kho **KinesisProducer** về.
46. Trong **Eclipse IDE**, Click **File**
* Click **Open projects from filesystem…**
47. Chọn đường dẫn đến project **KinesisProducer**
* Click **Finish**
48. Mở file **KinesisProducer\src\main\java\idevelop\demo\KinesisProducerApp.java**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037a.png?featherlight=false&width=90pc)
49. Tại dòng 69, thay **AP_SOUTHEAST_2** bằng **Region** của bạn
* Lưu lại
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037b.png?featherlight=false&width=90pc)
50. Mở **Command Prompt** và chuyển đường dẫn đến project **KinesisProducer**
* Chạy lệnh dưới đây để tạo một branch **master** mới
```
git checkout -b master
```
51. Chạy lệnh dưới đây để thêm các tập tin mới vào kho lưu trữ
```
git add .
```
52. Chạy lệnh dưới đây để commit tập tin mới
```
git commit -m "Initial commit"
```
53. Trong **Eclipse IDE**, chuột phải vào kho dữ liệu **KinesisProducer**
* Click **Push Branch \`master\`...**
54. Nhập thông tin **User** và **Password** đã lưu trong bước 41 vào mục **User** và **Password**
* Click **Login**
55. Click **Preview**
56. Click **Push**
57. Truy cập [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Nhập ```KinesisProducer``` vào ô tìm kiếm
* Click **KinesisProducer**
58. Bạn sẽ thấy các tập tin mà bạn đã thêm vào kho lưu trữ được liệt kê trong giao diện trình duyệt.

#### Sao chép kho lưu trữ vào Kinesis Producer EC2 instance

59. Mở **Command Prompt** và chạy lệnh dưới đây
```
chmod 400 <path to your key-pair file>
```
60. Chạy lệnh dưới đây
```
ssh -i <path to your key-pair file> ec2-user@<EC2InstanceKinesisProducer>
```
{{% notice note %}} 
Thay **\<EC2InstanceKinesisProducer\>** bằng giá trị **EC2InstanceKinesisProducer** trong tab **Output** của stack **DevAx-05**
{{% /notice %}}
61. Chạy lệnh dưới đây
```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
git clone https://git-codecommit.<your_region>.amazonaws.com/v1/repos/KinesisProducer
```
{{% notice note %}} 
Thay **<your_region>** bằng **Region** của bạn
{{% /notice %}}
* Nhập **User** và **Password** đã lưu trong bước 41 vào mục **Username** và **Password**
62. Chạy lệnh dưới đây
```
cd KinesisProducer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
63. Sẽ mất một chút thời gian để build mã nguồn ứng dụng.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-039.png?featherlight=false&width=60pc)
64. Producer cần dữ liệu nguồn và sẽ sử dụng một tập tin CSV trong thư mục làm việc hiện tại, được gọi là **SalesData.csv**
* Chạy lệnh dưới đây để sao chép tập tin vào EC2 instance
```
wget https://workshops.devax.academy/monoliths-to-microservices/module5/files/SalesData.csv
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-040.png?featherlight=false&width=60pc)

#### Cấu hình mapping index cho ElasticSearch

Khi dữ liệu được gửi đến ElasticSearch, nó sẽ cố gắng suy ra các kiểu dữ liệu trong nội dung và thường thực hiện tốt việc xác định đúng các kiểu dữ liệu. Trong trường hợp của chúng ta, chúng ta đang gửi ngày/giờ của mỗi bản ghi bán hàng dưới kiểu **long** và vì vậy ElasticSearch sẽ diễn giải trường này chỉ là một số (kiểu long). Thay vào đó, chúng ta cần cảnh báo ElasticSearch rằng nó sẽ diễn giải trường dấu thời gian này là **Date**. Chúng ta thực hiện việc này bằng cách tạo các Mapping trong chỉ mục trước khi chúng ta gửi bất kỳ bản ghi nào.
65. Trên cửa sổ dòng lệnh, bạn có kết nối SSH tới **Kinesis Consumer EC2 instance**, Chạy lệnh dưới đây
```
curl -X PUT "https://<ElasticsearchEndpoint>/transactions/" -k -d '{
	"mappings": {
		"transaction": {
			"properties": {
				"timestamp": {
					"type": "date"
				},
				"productLine": {
					"type": "string"
				},
				"retailerType": {
					"type": "string"
				},
				"productType": {
					"type": "string"
				},
				"product": {
					"type": "string"
				},
				"quantity": {
					"type": "long"
				},
				"unitCost": {
					"type": "double"
				},
				"totalCost": {
					"type": "double"
				}
			}
		}
	}
}';
```
66. Bạn sẽ nhận được thông báo ```{“acknowledged”:true}```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-041.png?featherlight=false&width=60pc)



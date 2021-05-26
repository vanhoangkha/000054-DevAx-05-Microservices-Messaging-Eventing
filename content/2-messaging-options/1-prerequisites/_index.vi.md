+++
title = "Các bước chuẩn bị"
date = 2021
weight = 1
chapter = false
pre = "<b>2.1. </b>"
+++

Trong bài thực hành, bạn có sự lựa chọn việc thực hiện trên môi trường Windows và Eclipse hoặc sử dụng môi trường phát triển trên Cloud sử udngj AWS Cloud9. AWS Cloud9 là một môi trường phát triển tích hợp (IDE) cho phép bạn viết, chạy và debug code của bạn trên trình duyệt.

{{% notice tip %}}
Nếu chưa sử dụng Cloud9 trước đó, đây là một cơ hội tốt để bạn sử nghiệm sử dụng môi trường IDE trên cloud! Bằng bất kì cách nào bạn chọn, bạn đều có thể thực hiện toàn bộ bài thực hành này.
{{% /notice %}}

**Nội dung**
- [Lựa chọn 1: Môi trường Eclipse](#lựa-chọn-1-môi-trường-eclipse)
- [Lựa chọn 2: Môi trường Cloud9](#lựa-chọn-2-môi-trường-cloud9)

#### Lựa chọn 1: Môi trường Eclipse

---
**Cài đặt và Cấu hình Instance - Phát triển trên Eclipse chạy Windows**

Trong bài thực hành, bạn sẽ kết nối tới "Máy ảo Windows". Bạn sẽ sử dụng máy ảo Windows được tạo từ Cloud Formation để thực hiện bài thực hành.
Một Windows EC2 instance đã được thiết lập cho bạn. Để truy cập vào được instance này, hãy thực hiện theo các bước trong [Phần 1.2. Môi trường Remote và Thiết lập AWS CLI](../../1-prerequisites/2-remote-env-config-cli/), sau đó thực hiện bài thực hành.

#### Lựa chọn 2: Môi trường Cloud9

---
**Sử dụng AWS Cloud9 làm môi trường Phát triển ứng dụng tích hợp**

Trong phần thực hành này, bạn sẽ sử dụng AWS Cloud9. AWS Cloud9 là một môi trường phát triển tích hợp dựa trên cloud (IDE) cho phép bạn viết, chạy và gỡ lỗi chỉ bằng một trình duyệt. Nó bao gồm một trình soạn thảo mã, trình gỡ lỗi và thiết bị đầu cuối. Cloud9 được đóng gói sẵn với các công cụ cần thiết cho các ngôn ngữ lập trình phổ biến, bao gồm JavaScript, Python, PHP, v.v., vì vậy bạn không cần phải cài đặt tệp hoặc định cấu hình máy phát triển của mình để bắt đầu các dự án mới. Vì Cloud9 IDE của bạn ở trên cloud, nên bạn có thể làm việc trên các dự án của mình từ văn phòng, nhà riêng hoặc bất cứ nơi nào bằng máy có kết nối internet. Cloud9 cũng cung cấp trải nghiệm đồng nhất trong việc phát triển các ứng dụng serverless cho phép bạn dễ dàng xác định tài nguyên, gỡ lỗi và chuyển đổi giữa thực thi local và remote của các ứng dụng serverless. Với Cloud9, bạn có thể nhanh chóng chia sẻ môi trường phát triển với nhóm của mình, cho phép bạn ghép nối chương trình và theo dõi đầu vào của nhau trong thời gian thực.

1. Từ thanh tìm kiếm **Services** - nhập vào ```Cloud9``` để truy cập console -> Chọn **Create environment**. Một trang mới xuất hiện để bạn nhập **Tên môi trường** và chọn **Next Step**.
2. Mục **Environment type**, chọn **Create a new EC2 Instance for environment (direct access)**
3. Mục **Instance type**, chọn a **t3.small**
4. Mục **Platform**, chọn **Amazon Linux 2**
5. Mục **VPC** chọn **DevAxNetworkVPC** (bạn có thể tìm kiếm VPC ID trong AWS Console ở dịch vụ VPC) và chọn subnet giống với máy **DevAxWindowsHost** trong danh sách máy ảo EC2. Chọn **Next step**.
6. Kiểm tra lại thiết lập và chọn **Create environment**. Sẽ mất một vài phút để tiến hành khởi tạo và bạn sẽ được chuyển vào giao diện **AWS Cloud9**.

![Cloud9](/images/2/0.png?width=90pc)

![Cloud9](/images/2/1.png?width=90pc)

7. Bắt đầu với việc tìm hiểu giao diện của Cloud9, Cloud9 cung cấp cho bạn một giao diện console để làm việc và bạn có thể gỡ lỗi và thiết lập lambda ở vùng bên tai phải. Kiểm tra xem các lambda function đã có trong tài khoản và xem xét chúng. 
8. Double click vào **CreateGitCredsFunction** và import chúng vào IDE. Bạn có thấy gì trong IDE không?

![Cloud9](/images/2/2.png?width=90pc)

    Bây giờ chúng ta cần thiết lập phiên bản JAVA cần thiết là 1.8 và thay đổi các thông số bắt buộc trên IDE. Chúng ta sẽ sử dụng Amazon Corretto, một distro miễn phí, multiplatform, production-ready của OpenJDK. Corretto đi kèm với long-term support sẽ có các cải tiến về hiệu suất và các bản sửa lỗi bảo mật.

9. Trong cửa sổ lệnh ở Cloud9, cài đặt Amazon Corretto 8 bằng các lệnh sau:
```bash
sudo amazon-linux-extras enable corretto8
sudo yum install -y java-1.8.0-amazon-corretto-devel
```

10. Trong cửa sổ lệnh ở Cloud9, kiểm tra phiên bản JAVA đã được cài đặt (ở lúc viết bài viết này là `1.8.0_265`):
```bash
java -version
```

Bạn sẽ thấy kết quả tương tự bên dưới:

```bash
Admin:~/environment $ 
openjdk version "1.8.0_265"
OpenJDK Runtime Environment Corretto-8.265.01.1 (build 1.8.0_265-b01)
OpenJDK 64-Bit Server VM Corretto-8.265.01.1 (build 25.265-b01, mixed mode)
```

---
**Cài đặt SAM CLI**

Chúng ta sẽ cài đặt AWS Serverless Application Model (SAM) CLI vào môi trường AWS Cloud9 của chúng ta. AWS Serverless Application Model (SAM) là một open-source framework để xây dựng các ứng dụng serverless. Nó cung cấp cú pháp viết tắt để diễn đạt các hàm, API, cơ sở dữ liệu và ánh xạ nguồn sự kiện. Chỉ với một vài dòng trên mỗi tài nguyên, bạn có thể xác định ứng dụng bạn muốn và lập model ứng dụng đó bằng YAML. Trong quá trình triển khai, SAM chuyển đổi và mở rộng cú pháp SAM thành cú pháp AWS CloudFormation, cho phép bạn xây dựng các ứng dụng serverless nhanh hơn.

11. Trong cửa sổ lệnh ở Cloud9, cài đặt SAM CLI với câu lệnh sau: 
```bash
sudo pip3 install --upgrade aws-sam-cli
```

---
**Cài đặt maven**

Chúng ta sẽ sử dụng Apache Maven để quản lý việc xây dựng dự án và quản lý các thư viện cho dự án của chúng tôi.

12.  Trong cửa sổ lệnh ở Cloud9, cài đặt Apache Maven với câu lệnh sau: 
```bash
sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
sudo yum install -y apache-maven
```

Không may, trình cài đặt maven chuyển JDK của chúng ta thành 1.7, vì vậy chúng ta cần chuyển về 1.8 9.

13. To switch our JDK to Corretto 8, issue the following commands in the Cloud9 terminal:
```bash
sudo alternatives --config java #enter the number for corretto 8
```

```bash
sudo alternatives --config javac #enter the number for corretto 8
```

Bạn sẽ thấy một cái gì đó tương tự như sau:

![Cloud9](/images/2/3.png?width=90pc)

Bây giờ bạn có thể sử dụng Cloud9 để tiếp tục bài thực hành.
+++
title = "Chuẩn bị môi trường"
weight = 1
chapter = false
pre = "<b>1.1. </b>"
+++

Trong bài thực hành này, chúng ta sẽ sử dụng CloudFormation template đã cung cấp và cài đặt sẵn các tài nguyên cần thiết.

Để triển khai ở tài khoản của bạn, bạn có thể tải tập tin bên dưới và triển khai ở dịch vụ CloudFormation:
{{%attachments title="CloudFormation Template" pattern=".*(yaml)"/%}}

Mô hình bên dưới thể hiện các tài nguyên được tạo khi triển khai CloudFormation Template:

![Diagram](/images/1/0.png?width=50pc)

**Nội dung**
- [Khởi tạo Keypair](#khởi-tạo-keypair)
- [Triển khai CloudFormation Template](#triển-khai-cloudformation-template)
- [VPC](#vpc)
- [EC2](#ec2)
- [RDS](#rds)

#### Khởi tạo Keypair
{{% notice tip %}}
Nếu bạn đã có Keypair được tạo, bạn có thể bỏ qua bước này.
{{% /notice %}}

1. Trước tiên, cần tạo một Keypair cho các máy ảo trong bài thực hành. Truy cập EC2 console, chọn **Keypairs**
2. Chọn **Create key pair**
![Keypair](/images/1/1.png?width=90pc)
3. Nhập tên keypair **KPforDevAxInstances**, chọn định dạng tập tin **pem** và chọn **Create key pair**
![Keypair](/images/1/2.png?width=90pc)
4. Một cửa sổ xuất hiện để chọn nơi lưu trữ tập tin keypair. Bạn cần lưu trữ keypair này để truy cập vào máy ảo EC2 trong các bước sau

#### Triển khai CloudFormation Template

1. Truy cập **AWS CloudFormation**
![CloudFormation](/images/1/3.png?width=90pc)
2. Chọn **Create stack**, chọn **With new resources (standard)**
![CloudFormation](/images/1/4.png?width=90pc)
3. Tại mục **Prerequisite - Prepare template** chọn **Template is ready**
4. Tại mục **Template source**, chọn **Upload a template file**, nhấp chọn **Choose file** và trỏ tới file template đã tải xuống. Chọn **Next**
![CloudFormation](/images/1/5.png?width=90pc)
5. Nhập tên stack tại mục **Stack name** (VD: ```DevAx-M2M-M5```)
6. Chọn keypair **KPforDevAxInstances** cho mục **EEKeyPair** và chọn **Next**
![CloudFormation](/images/1/6.png?width=90pc)
7. Chọn **Next** tại trang **Configure stack options**
![CloudFormation](/images/1/7.png?width=90pc)
8.  Chọn **Create stack**
![CloudFormation](/images/1/8.png?width=90pc)
9. Chúng ta cần chờ một vài phút để các tài nguyên được khởi tạo và cấu hình.

#### VPC

CloudFormation template định nghĩa một VPC gọi là DevAxNetworkLabVPC chứa 4 mạng con (network subnet) - 2 công khai (các máy ảo được gán với mạng con này có thể được truy cập trực tiếp từ internet) và 2 riêng tư (các mảy ảo được gán với mạng con nào sử dụng private IP và không thể được truy cập trực tiếp từ internet)
1. Trong AWS Console, mở **VPC**
2. Trong bảng điều khiển **VPC**, chọn **Your VPCs**, ta thấy một VPC được tạo có tên **CdkStack/DevAxNetworkLabVPC**.

[VPC](/images/1/9.png?width=90pc)

3. Chọn **Subnets** và Filter với VPC *CdkStack/LabVPC* bạn sẽ thấy 4 subnets - 2 private và 2 public. 

![VPC](/images/1/10.png?width=90pc)

Bài thực hành này sử dụng 2 Availibility Zones (AZs), mỗi AZs có một public và một private subnet.
CloudFormation template cũng định nghĩa một Network Address Translation gateway (NAT gateway) thực hiện ánh xạ giữa địa chỉ IP nội bộ của các máy chủ định tuyến qua nó và địa chỉ IP bên ngoài, để các máy ảo dù có private IP nhưng vẫn có thể truy xuất dữ liệu từ Internet.
4. Chọn **NAT Gateways**. Bạn sẽ thấy 2 NAT gateways, mỗi gateway cho một private subnet truy cập internet và các dịch vụ khác của AWS nhưng ngăn các dịch vụ bên ngoài truy cập trực tiếp với các tài nguyên bên trong các private subnet

![VPC](/images/1/11.png?width=90pc)


#### EC2

CloudFormation cũng tạo một **Workspace instance**. 
1. Truy cập vào bảng điều khiển EC2
2. Chọn **Instances**, bạn sẽ thấy một EC2 instance đã được tạo sẵn

![EC2](/images/1/12.png?width=90pc)
{{%notice warning%}}
Nếu bạn không thấy bất kỳ instance nào trong danh sách, vui lòng kiểm tra bảng điều khiển AWS CloudFormation. Các mod-xxxxxx stack status, phải là CREATE_COMPLETE trước khi một EC2 instance chạy
{{%/notice%}}

3. Chọn tab **Security**, bạn sẽ thấy security group được gắn với máy ảo.

![EC2](/images/1/13.png?width=90pc)

4. Chọn mở rộng tab **Inbound rules** bạn sẽ thấy các giao thức cũng như địa chỉ IP nguồn được phép truy cập từ những port này.

![EC2](/images/1/14.png?width=90pc)

Trong bài thực hành này, CloudFormation template tạo các quy tắc bảo mật cho phép tất cả các địa chỉ IP nguồn được phép kết nối tới. Bạn có thể chỉnh sửa và chỉ cho phép kết nối tới từ địa chỉ IP của bạn.

5. Chọn tab **Volumes**, bạn sẽ thấy một EBS volume.

![EC2](/images/1/15.png?width=90pc)

#### RDS

Dịch vụ cơ sở dữ liệu quan hệ của Amazon (Amazon RDS) giúp dễ dàng thiết lập, vận hành và mở rộng quy mô cơ sở dữ liệu quan hệ trên đám mây. Nó cung cấp khả năng tiết kiệm chi phí và có thể thay đổi kích thước trong khi tự động hóa các tác vụ quản trị tốn thời gian như cung cấp phần cứng, thiết lập cơ sở dữ liệu, vá lỗi và sao lưu.

1. Truy cập bảng điều khiển RDS
2. Chọn **Database**
3. Bạn sẽ thấy một database mới được tạo, chọn vào database này
4. Database được tạo đã được cấu hình với các dịch vụ VPC, Security Groups,...

![RDS](/images/1/16.png?width=90pc)
+++
title = "VPC"
weight = 1
chapter = false
pre = "<b>1.1.1. </b>"
+++

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

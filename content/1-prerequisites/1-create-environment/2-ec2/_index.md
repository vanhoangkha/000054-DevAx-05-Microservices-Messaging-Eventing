+++
title = "EC2"
weight = 2
chapter = false
pre = "<b>1.1.2. </b>"
+++

#### EC2

CloudFormation cũng tạo một workspace instance. 
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

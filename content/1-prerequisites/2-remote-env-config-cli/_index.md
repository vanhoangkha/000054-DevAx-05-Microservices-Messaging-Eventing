+++
title = "Remote enviroment & Configure the AWS CLI"
weight = 2
chapter = false
pre = "<b>1.2. </b>"
+++

#### Remote into Windows instance
1. Truy cập vào AWS EC2 và vào máy ảo DexAxWindowsHost
2. Chọn **Connect**

![ConnectEC2](/images/1/17.png?width=90pc)

3. Chọn tab **RDP client**

![ConnectEC2](/images/1/18.png?width=90pc)

4. Chọn **Getpassword** để lấy mật khẩu đăng nhập vào máy ảo
5. Chọn **Browse** và trỏ tới tập tin keypair đã lưu ở phần 1. Chọn **Decrypt Password**

![ConnectEC2](/images/1/19.png?width=90pc)

6. Sao chép mật khẩu và chọn **Download remote desktop file**. Một file RDP sẽ được tải xuống, dùng để truy cập vào máy ảo Windows

![ConnectEC2](/images/1/20.png?width=90pc)

7. Mở file RDP đã tải, nhập mật khẩu đã lưu ở bước 6 và chọn Connect. Một cảnh báo về xác minh chứng chỉ có thể xuất hiện, chọn **Continue** để tiếp tục.

Như vậy, ta đã kết nối được vào máy ảo Windows được tạo.

#### Configure AWS CLI

1. Mở và nhập đoạn sau vào trình soạn thảo

```bash
aws configure set profile.devaxacademy.region <your_region>
aws configure set profile.devaxacademy.aws_access_key_id <access_key_id>
aws configure set profile.devaxacademy.aws_secret_access_key <secret_access_key>
```

2.  Nếu bạn đã có một bộ thông tin xác thực cho người dùng IAM có đặc quyền quản trị, bạn có thể sử dụng lại chúng cho các thông tin bên dưới trong bước tiếp theo. Nếu bạn chưa có thông tin xác thực truy cập, làm theo hướng dẫn [tại đây](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) để truy xuất chúng cho người dùng có đặc quyền quản trị để sử dụng trong bài thực hành này.
3. Điền các giá trị của người dùng IAM vào câu lệnh aws configure:
    - \<your_region\> với **Region code**
    - \<access_key_id\> với giá trị **Access Key Id**
    - \<secret_access_key\> với giá trị **Secret Access Key**
4. Sao chép và dán đoạn code vào comand line và thực thi chúng
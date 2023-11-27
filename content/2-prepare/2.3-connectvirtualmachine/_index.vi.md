+++
title = "Kết nối Windows instance"
date = 2020
weight = 3
chapter = false
pre = "<b>2.3. </b>"
+++
#### Kết nối Windows instance
1. Truy cập [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Trên thanh điều hướng bên trái, chọn **Intances**.
* Nhập ```DevAxWindowsHost``` vào ô tìm kiếm và nhấn **Enter**
* Chọn **DevAxWindowsHost**.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-001.png?featherlight=false&width=90pc)
2. Tại trang **Connect to instance**
* Click tab **RDP client**.
* Click **Download remote desktop file**. Chúng ta sẽ download file remote desktop xuống đường dẫn thư mục chứa key pair.
* Click **Get password**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-002.png?featherlight=false&width=90pc)
3. Tại trang **Get Windows password**:
* Click **Browse**. 
* Chọn file **KPforDevAxInstances.pem** đã tải về trong phần 2.1.
* Click **Decrypt Password** để giải mã thông tin password. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-003.png?featherlight=false&width=90pc)
4. Copy password đã được giải mã.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-004.png?featherlight=false&width=90pc)
5. Mở file **DevAxWindowsHost.rdp** bạn đã tải về trong bước 2.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-005.png?featherlight=false&width=30pc)
6. Điền thông tin password đã copy trong bước 4
* Click **OK**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-006.png?featherlight=false&width=25pc)
7. Click **Don’t ask me again for connections to this computer**.
* Click **Yes**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-007.png?featherlight=false&width=25pc)
8. Bạn đã kết nối thành công. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-008.png?featherlight=false&width=90pc)

#### Cấu hình AWS CLI
1. Gán quyền Administrator Access vào user **awsstudent** được tạo bởi Cloud Formation template
* Truy cập vào [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Click user **awsstudent**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-009.png?featherlight=false&width=90pc)
2. Trong phần **Permissions policies**
* Click **Add permissions**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-010.png?featherlight=false&width=90pc)
3. Trong trang **Add permissions to awsstudent**
* Click **Attach existing policies directly**
* Gõ ```AdministratorAccess``` vào ô tìm kiếm.
* Chọn **AdministratorAccess**
* Click **Next:Review**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-011.png?featherlight=false&width=90pc)
4. Click **Add Permission**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-012.png?featherlight=false&width=90pc)
5. Click tab **Security credentials**
* Click **Create access key** để tiến hành tạo access key
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-013.png?featherlight=false&width=90pc)
6. Lưu thông tin **Access key** và **Secret access key** để phục vụ cho bước tiếp theo
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-014.png?featherlight=false&width=90pc)
7. Chạy lệnh dưới đây
```
aws configure set profile.aws-lab-env.region <YOUR_REGION>
aws configure set profile.aws-lab-env.aws_access_key_id <YOUR_ACCESS_KEY_ID>
aws configure set profile.aws-lab-env.aws_secret_access_key <YOUR_SECRET_ACCESS_KEY>
git config --global user.email <YOUR_EMAIL>
git config --global user.name awsstudent
```
{{% notice note %}} 
Thay **<your_region>** bằng **Region code** \
Thay **<access_key_id>** bằng **Access Key Id** bạn đã lưu về trong bước 6\
Thay **<secret_access_key>** bằng **Secret Access Key** bạn đã lưu về trong bước 6
{{% /notice %}}
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-015.png?featherlight=false&width=60pc)
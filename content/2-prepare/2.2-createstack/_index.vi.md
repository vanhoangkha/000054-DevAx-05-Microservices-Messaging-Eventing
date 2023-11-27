+++
title = "Tạo CloudFormation stack"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++
#### Tạo CloudFormation stack
Chúng ta sẽ dùng tập tin template dưới đây để tạo CloudFormation stack.

{{%attachments title="Template File" style="orange" pattern="Module5.template.yaml"/%}}
1. Tải tệp tin **Module5.template.yaml**.
2. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Click **Create stack**.
* Click **With new resources (standard)**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-001.png?featherlight=false&width=90pc)
3. Trong phần **Specify template**.
* Chọn **Upload a template file**
* Click **Choose file**, sau đó chọn tệp tin **Module5.template.yaml** chúng ta đã tải về.
* Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-002.png?featherlight=false&width=90pc)
4. Trong phần **Stack name** gõ ```DevAx-05```.
* Trong phần **EEKeyPair** chọn **KPforDevAxInstances**.
* Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-003.png?featherlight=false&width=90pc)
5. Tại trang **Configure stack options**, Kéo màn hình xuống dưới sau đó Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-004.png?featherlight=false&width=90pc)
6. Tại trang **Review**.
* Kéo màn hình xuống dưới sau đó Click **I acknowledge that AWS CloudFormation might create IAM resources with custom names.**
* Click **Create stack**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-005.png?featherlight=false&width=90pc)
{{% notice note %}} 
**Cloudformation** sẽ mất khoảng 5 phút để tạo stack . Hãy đợi cho đến khi tất cả các stack ở trạng thái **CREATE_COMPLETE**.
{{% /notice %}}
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-006.png?featherlight=false&width=90pc)
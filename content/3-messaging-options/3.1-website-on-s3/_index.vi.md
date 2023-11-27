+++
title = "Triển khai Web trên S3"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++
#### Tạo, cấu hình và tải website vào S3 bucket 
1. Truy cập vào máy ảo Windows. Mở **Command Prompt**
* Chạy lệnh dưới đây để tạo một Amazon S3 bucket để lưu mã nguồn website.
```
aws s3 mb s3://idevelop-lab5-<your_initials> --region <your_region> --profile aws-lab-env
```
{{% notice note %}} 
Thay **<your_initials>** bằng tên của bạn hoặc một số ký tự khác để tạo một Amazon S3 bucket có tên duy nhất.\
Thay **<your_region>** bằng **Region code** của bạn
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-001.png?featherlight=false&width=60pc)
{{% notice tip %}} 
Bạn có thể tạo một Amazon S3 bucket để lưu mã nguồn website bằng AWS Console hoặc bằng giao diện dòng lệnh.
{{% /notice %}}
{{%attachments title="Messaging Project" style="orange" pattern="Messaging.zip"/%}}
2. Tải file **Messaging.zip** và giải nén
* Mở **Messaging Project** trong Eclipse IDE
* Mở file có đường dẫn **Messaging/scripts/aws-config.js**
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-002.png?featherlight=false&width=90pc)
3. Chạy lệnh dưới đây
```
aws cloudformation describe-stacks --stack-name DevAx-05 --query "Stacks[].Outputs[*].[OutputKey,OutputValue]" --profile aws-lab-env
```
* Lưu lại thông tin **CognitoIdentityPoolId**, **QueueUrl1**, **QueueUrl2**, **QueueUrl3**, **FIFOQueueUrl**, **TopicARN** để dùng trong bước tiếp theo
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-003.png?featherlight=false&width=60pc)
4. Trong file **aws-config.js**, thay thế **ap-southeast-2** bằng **Region Code** của bạn
* Thay thế **REPLACE_WITH_COGNITO_IDENTITY_POOL_ID** bằng giá trị của **CognitoIdentityPoolId** đã lưu trong bước 3
* Thay thế **REPLACE_WITH_QUEUE_URL_1** bằng giá trị của **QueueUrl1** đã lưu trong bước 3
* Thay thế **REPLACE_WITH_QUEUE_URL_2** bằng giá trị của **QueueUrl2** đã lưu trong bước 3
* Thay thế **REPLACE_WITH_QUEUE_URL_3** bằng giá trị của **QueueUrl3** đã lưu trong bước 3
* Thay thế **REPLACE_WITH_QUEUE_URL_FIFO** bằng giá trị của **FIFOQueueUrl** đã lưu trong bước 3
* Thay thế **REPLACE_WITH_SNS_TOPIC_ARN** bằng giá trị của **TopicARN** đã lưu trong bước 3
* Lưu lại.
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-004.png?featherlight=false&width=90pc)
5. Có một mục khác cần thay thế, **REPLACE_WITH_IOT_ENDPOINT**. Chạy lệnh dưới đây để xác định giá trị này
```
aws iot describe-endpoint --endpoint-type iot:Data-ATS --region <your_region> --profile aws-lab-env
```
{{% notice note %}} 
Thay **<your_region>** bằng **Region code** của bạn
{{% /notice %}}
* Lưu lại giá trị **endpointAddress** để sử dụng trong bước tiếp theo
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-005.png?featherlight=false&width=60pc)
6. Trong file **aws-config.js**, thay thế **REPLACE_WITH_IOT_ENDPOINT** bằng giá trị của **endpointAddress** đã lưu trong bước 5
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-006.png?featherlight=false&width=90pc)
7. Chạy lệnh dưới đây
```
aws s3 website s3://<bucket_name> --index-document sqs-standard.html --region <your_region> --profile aws-lab-env
```
{{% notice note %}} 
Thay **<bucket_name>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.\
Thay **<your_region>** bằng **Region code** của bạn.
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-007.png?featherlight=false&width=60pc)

{{% notice warning %}} 
Từ **Bước 8** đến **Bước 10**, Public policy sẽ được áp dụng cho bucket của bạn chỉ dành cho mục đích thực hành và đây là một rủi ro bảo mật. Nếu bạn không muốn công khai bucket của mình và chỉ muốn công khai các đối tượng. Vui lòng chuyển sang **Bước 11**.
{{% /notice %}}

8. Trong Eclipse IDE, mở file có đường dẫn **Messaging/bucket/policy.json**
* Thay **idevelop-lab5-< yourinitials>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.
* Lưu lại.
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-008.png?featherlight=false&width=90pc)
9. Trong Eclipse IDE, click **Messaging**
* Nhấn tổ hợp phím **Ctrl+Alt+T** để mở terminal
* Chạy lệnh dưới đây để cập nhật **bucket policy**
```
aws s3api put-bucket-policy --bucket <bucket_name> --policy file://bucket/policy.json --region <your_region> --profile aws-lab-env
```
{{% notice note %}} 
Thay **<bucket_name>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.\
Thay **<your_region>** bằng **Region code** của bạn.
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-009.png?featherlight=false&width=90pc)
10. Chạy lệnh dưới đây để sao chép nội dung trong thư mục mà bạn đã giải nén lên trên S3 bucket mà bạn đã tạo
```
aws s3 cp . s3://<bucket_name> --recursive --region <your_region> --profile aws-lab-env
```
{{% notice note %}} 
Thay **<bucket_name>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.\
Thay **<your_region>** bằng **Region code** của bạn.
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-010.png?featherlight=false&width=90pc)
11. Chạy lệnh dưới đây để sao chép nội dung trong thư mục mà bạn đã giải nén và mở cho phép truy cập public tất cả các tập tin đó lên trên S3 bucket mà bạn đã tạo
```
aws s3 cp . s3://<bucket_name> --recursive --region <your_region>  --profile aws-lab-env --acl public-read
```
{{% notice note %}} 
Thay **<bucket_name>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.\
Thay **<your_region>** bằng **Region code** của bạn.
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-011.png?featherlight=false&width=90pc)
12. Sau khi lệnh thực thi, bạn đã có thể xem lại trang web trong trình duyệt của mình. Mở trình duyệt web và truy cập theo link dưới đây
```
http://<bucket_name>.s3-website-<your_region>.amazonaws.com/sqs-standard.html
```
{{% notice note %}} 
Thay **<bucket_name>** bằng tên của S3 bucket chúng ta đã tạo trong bước 1.\
Thay **<your_region>** bằng **Region code** của bạn.
{{% /notice %}}
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-012.png?featherlight=false&width=90pc)

#### Thiết lập SQS Queue

Thiết lập môi trường bài thực hành đã cấu hình hàng đợi SQS được sử dụng trong bài thực hành này, sử dụng CloudFormation template.

1. Truy cập [**AWS SQS console**](https://console.aws.amazon.com/sqs/home).
![Setup S3 bucket website](/images/3-messaging-options/3.1-website-on-s3/website-on-s3-013.png?featherlight=false&width=90pc)

Bốn hàng đợi đã được thiết lập. Ba là hàng đợi tiêu chuẩn và một là hàng đợi FIFO. Đây là một đoạn mã của CloudFormation template đã thiết lập các tài nguyên này:
```
"idevelopDrawQueueFIFO": {
	"Properties": {
		"QueueName": "idevelop_drawqueue.fifo",
		"FifoQueue": true,
		"ContentBasedDeduplication": true
	},
	"Type": "AWS::SQS::Queue"
},

"idevelopDrawQueueStandard1": {
	"Properties": {
		"QueueName": "idevelop_drawqueue_1"
	},
	"Type": "AWS::SQS::Queue"
}
```
Như bạn có thể thấy, rất dễ dàng sử dụng CloudFormation để khai báo các hành đợi SQS của bạn. Bạn cũng có thể sử dụng nút **Create New Queue** trong AWS Console để tạo hàng đợi theo cách thủ công.



+++
title = "Triển khai Web trên S3"
date = 2021
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++

Trong bài tập này, bạn sẽ thiết lập một trang web được host trên Amazon S3. Trang web sẽ hiển thị các phương thức truyền tin khác nhau, được sắp xếp trong trình duyệt web. Trong bài thực hành, bạn sẽ được hướng dẫn thông qua việc tìm hiểu cấu hình AWS mà bài thực hành đã thiết lập cho bạn.

**Nội dung**
- [Tạo, cấu hình và tải website vào S3 bucket](#tạo-cấu-hình-và-tải-website-vào-s3-bucket)
- [Thiết lập SQS Queue](#thiết-lập-sqs-queue)

#### Tạo, cấu hình và tải website vào S3 bucket

1. Tạo một Amazon S3 bucket để lưu mã nguồn website.

Bạn có thể thực hiện việc này từ AWS Console hoặc từ giao diện dòng lệnh, chạy lệnh sau từ cửa số lệnh hoặc từ Cloud9:

```bash
aws s3 mb s3://idevelop-lab5-<yourinitials> --region ap-northeast-1
```

{{% notice note %}}
Nếu bạn đang sử dụng Máy ảo Windows và Eclipse, đừng quên thêm```--profile aws-lab-env```
{{% /notice %}}

Tên bucket phải là duy nhất trên toàn cầu, vì vậy chúng tôi khuyên bạn nên thêm vào tên chuỗi đặc biệt của bạn trong tên bucket để làm cho nó trở nên duy nhất.

{{% notice warning %}}
Nếu bạn nhận được lỗi rằng tên bucket không khả dụng, hãy sử dụng một số ký tự phụ sau chuỗi đặc biệt của bạn để tạo một bucket **có tên duy nhất**.
{{% /notice %}}

{{% notice note %}}
Hãy nhớ **tên** bạn sử dụng cho S3 bucket vì bạn sẽ cần tham chiếu đến tên bucket này sau này trong bài thực hành.
{{% /notice %}}

1. Bây giờ hãy chọn môi trường thực hiện phù hợp với bạn:
   1. **CLOUD9**
  Nếu bạn đang sử dụng môi trường Cloud9, hãy làm như sau để triển khai:
    
  **Lựa chọn 1:** Tải về [Messaging.zip](https://workshops.devax.academy/monoliths-to-microservices/module5/files/Messaging.zip). Trên Cloud9 **File -> Upload Local Files…**   

  **Lựa chọn 2:** Tải về tập tin bên dưới. Trên Cloud9 **File -> Upload Local Files…**  

{{%attachments title="Application file" pattern=".*(zip)"/%}}

![SQS](/images/2/4.png?width=90pc)

  **Lựa chọn 3:** Sử dụng cửa sổ dòng lệnh trong Cloud9 để tải về

  ```bash
  wget -L http://workshops.devax.academy/monoliths-to-microservices/module5/files/Messaging.zip
  ```

   2. **ECLIPSE**
    
  **Lựa chọn 1:** Tải xuống gói mã nguồn [Messaging.zip](https://workshops.devax.academy/monoliths-to-microservices/module5/files/Messaging.zip) và giải nén ra thư mục làm việc của bạn. Trong Eclipse **File -> Upload project from your filesystem...** . Đặt các tập tin đã giải nén vào thư mục và import vào Eclipse như đã thực hiện trước đó.
  
  **Lựa chọn 2:** Tải xuống tệp bên dướivà giải nén ra thư mục làm việc của bạn. Trong Eclipse **File -> Upload project from your filesystem...** . Đặt các tập tin đã giải nén vào thư mục và import vào Eclipse như đã thực hiện trước đó.

{{%attachments title="Application file" pattern=".*(zip)"/%}}

3. Sử dụng IDE mà bạn chọn và tìm tới tập tin ```<path_to_exploded_zip>/scripts/aws-config.js```. Tập tin có các nội dung sau:

{{% notice note %}}
Lưu ý **Region** trong tập lệnh cấu hình - thay đổi thành khu vực mà bạn đang sử dụng.
{{% /notice %}}

```js
var config = {
  Region: "ap-northeast-1",

  Cognito: {
    IdentityPoolId: "REPLACE_WITH_COGNITO_IDENTITY_POOL_ID",
  },

  SQS: {
    Standard: {
      QueueUrl: "REPLACE_WITH_QUEUE_URL_1",
    },
    Secondary: {
      QueueUrl: "REPLACE_WITH_QUEUE_URL_2",
    },
    Tertiary: {
      QueueUrl: "REPLACE_WITH_QUEUE_URL_3",
    },
    FIFO: {
      QueueUrl: "REPLACE_WITH_QUEUE_URL_FIFO",
      GroupId: "idevelop_sqs_fifo",
    },
  },

  SNS: {
    TopicARN: "REPLACE_WITH_SNS_TOPIC_ARN",
  },

  Kinesis: {
    StreamName: "iDevelopDrawingData",
    PartitionKey: "Partition1",
  },

  IoT: {
    Endpoint: "REPLACE_WITH_IOT_ENDPOINT",
    Topic: "idevelop/drawingdemo",
  },
};
```
Lưu ý rằng có một số giá trị **REPLACE_WITH_XXX**. Bạn cần cập nhật tập tin này và thay thế các trình giữ chỗ đó bằng ARN và URL chính xác từ môi trường bài thực hành của bạn. Bảng bên dưới hiển thị các giá trị cần được thay thế và bạn có thể tìm các giá trị để thay thế trong console của bài thực hành, chúng ở trong **Output** của **Cloudformation stack** ban đầu được sử dụng để cài đặt môi trường cho bạn.

Mở một cửa sổ lệnh trong IDE của bạn và chạy phần sau để sử dụng AWS CLI để nhận các kết quả đầu ra được yêu cầu bên dưới:

Hãy nhớ rằng bạn sẽ cần sử dụng `--profile aws-lab-env` nếu bạn đang ở trên máy chủ Windows. Nếu bạn đang sử dụng Cloud9, bước này sẽ sử dụng AWS Role để cho phép bạn thực hiện việc này, vì vậy nó có quyền trên các EC2 cloud9 instance.

```bash
export dets=$(aws cloudformation describe-stacks --query "Stacks[?StackName>'mod'].StackName" --output text)
echo $dets
```

```bash
aws cloudformation describe-stacks --stack-name $dets --query "Stacks[].Outputs[*].[OutputKey,OutputValue]"
```

Lưu ý rằng điều này cung cấp tất cả các chi tiết bạn cần ở trên. Hãy để lấy ra các thông tin bạn muốn bằng cách sử dụng shell:

```bash
aws cloudformation describe-stacks --stack-name $dets --query "Stacks[].Outputs[?OutputKey=='QueueUrl2'][OutputValue]" --output text
```

Dưới đây là một lệnh có thể tái sử dụng trong tương lai khi bạn cần dùng trong terminal để có được các thông số

```bash
cat << EOF > list
CognitoIdentityPoolId
QueueUrl1
QueueUrl2
QueueUrl3
FIFOQueueUrl
TopicARN
EOF
```

Thao tác này sẽ tạo ra một tập tin danh sách các biến mà chúng ta có thể đi tuần tự qua:

```bash
for i in `cat list`
do
echo $i
aws cloudformation describe-stacks --stack-name $dets --query "Stacks[].Outputs[?OutputKey=='$i'][OutputValue]" --output text
done
```

Đầu ra sẽ như thế này:

```bash
CognitoIdentityPoolId
ap-northeast-1:dXXX-XXX0-4XXX-XXX-dXXXX
QueueUrl1
https://sqs.ap-northeast-1.amazonaws.com/XXXX/idevelop_drawqueue_1
QueueUrl2
https://sqs.ap-northeast-1.amazonaws.com/XXXX/idevelop_drawqueue_2
QueueUrl3
https://sqs.ap-northeast-1.amazonaws.com/XXXX/idevelop_drawqueue_3
FIFOQueueUrl
https://sqs.ap-northeast-1.amazonaws.com/XXXX/idevelop_drawqueue.fifo
TopicARN
arn:aws:sns:ap-northeast-1:XXXX:iDevelopthông tinDuplicator
```

| Replace this placeholder…             | With this the value with the label.. |
|---------------------------------------|--------------------------------------|
| REPLACE_WITH_COGNITO_IDENTITY_POOL_ID | CognitoIdentityPoolId                |
| REPLACE_WITH_QUEUE_URL_1              | QueueUrl1                            |
| REPLACE_WITH_QUEUE_URL_2              | QueueUrl2                            |
| REPLACE_WITH_QUEUE_URL_3              | QueueUrl3                            |
| REPLACE_WITH_QUEUE_URL_FIFO           | FIFOQueueUrl                         |
| REPLACE_WITH_SNS_TOPIC_ARN            | TopicARN                             |

{{% notice note %}}
Quá trình thiết lập bài thực hành sẽ mất vài phút để hoàn tất và có thể chưa hoàn tất khi bạn xem phần này của hướng dẫn bài thực hành. Nếu trường hợp như vậy, bạn có thể đợi thiết lập bài thực hành hoàn tất hoặc bạn có thể truy xuất các giá trị ở trên từ tab **Resources** của bảng điều khiển **CloudFormation**, cho stack đang tạo các tài nguyên này . Xem liệu bạn có thể xác định được các giá trị liên quan hay không và tự hoàn tất việc điền vào cấu hình - hoặc đợi quá trình thiết lập bài thực hành hoàn tất.
{{% /notice %}}

4. Có một mục khác cần thay thế, **REPLACE_WITH_IOT_ENDPOINT**. Bạn cần truy vấn môi trường AWS để xác định giá trị này. Trong cửa sổ dòng lệnh, hãy sử dụng lệnh sau:

```bash
aws iot describe-endpoint --endpoint-type iot:Data-ATS --region ap-northeast-1 --profile aws-lab-env
```

Bạn sẽ thấy một phản hồi tương tự như sau:

```json
{
  "endpointAddress": "a33e9q9lyxxxxx-ats.iot.ap-northeast-1.amazonaws.com"
}
```

5. Sao chép giá trị endpointAddress từ cửa sổ dòng lệnh của bạn và thay thế giá trị **REPLACE_WITH_IOT_ENDPOINT** trong tập tin ***aws-config.js*** bằng giá trị của bạn. 
6. Lưu các thay đổi bạn đã thực hiện vào tập tin ***aws-config.js***.
7. Để S3 lưu trữ các tập tin giống như một web server, chúng ta cần bật chế độ *website* trên S3 bucket. Chúng tôi có thể thực hiện việc này từ AWS console hoặc cửa sổ dòng lệnh - ở đây chúng ta sẽ sử dụng dòng lệnh:

```bash
aws s3 website s3://idevelop-lab5-<yourinitials> --index-document sqs-standard.html --region ap-northeast-1 --profile aws-lab-env
```

{{% notice warning %}}
Từ **Bước 8** đến **Bước 11A**, Public policy sẽ được áp dụng cho bucket của bạn chỉ dành cho **mục đích thực hành** và đây là một **rủi ro bảo mật**.
Nếu bạn không muốn công khai bucket của mình và chỉ muốn công khai các đối tượng. Vui lòng chuyển sang **Bước 11B**.
{{% /notice %}}

8. Cuối cùng, bạn cần thêm *bucket policy* vào  S3 bucket để cho phép những người yêu cầu ẩn danh đọc các đối tượng trong bucket. Policy đã được cung cấp cho bạn trong tập tin **bucket/policy.json**, nhưng cần được chỉnh sửa để cập nhật tên bucket cho khớp với tên bucket của bạn. 

Sử dụng **Eclipse IDE** hoặc **Cloud9** để mở tập tin `<path_to_exploded_zip>/bucket/policy.json`. Tập tin có các nội dung sau: 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::idevelop-lab5-<yourinitials>/*"]
    }
  ]
}
```

Cập nhật tập tin và thay thế ```idevelop-lab5-<yourinitials>``` bằng tên bucket của bạn. Đảm bảo rằng **/*** vẫn ở cuối tên bucket. Điều này rất quan trọng vì nó chỉ định rằng tất cả các tập tin bên dưới thư mục gốc của nhóm đều có thể truy cập được thông qua *Action* **s3:GetObject**. 

9. Lưu tập tin.
10. Trong cửa sổ lệnh, `cd` vào thư mục mà bạn đã giải nén tập tin zip.

Để cập nhật **bucket policy**, sử dụng cửa sổ lệnh với lệnh sau:

```bash
aws s3api put-bucket-policy --bucket idevelop-lab5-<yourinitials> --policy file://bucket/policy.json --region ap-northeast-1 --profile aws-lab-env
```

{{% notice note %}}
Chắc chắn bạn thay thế tên bucket trong đoạn mã với tên bucket của bạn.
{{% /notice %}}

11. **A.** Sao chép nội dung trong thư mục mà bạn đã giải nén lên trên S3 bucket mà bạn đã tạo, sử dụng câu lệnh sau: 

```bash
aws s3 cp . s3://idevelop-lab5-<yourinitials> --recursive --region ap-northeast-1 --profile aws-lab-env
```

11. **B.** Sao chép nội dung trong thư mục mà bạn đã giải nén và **mở cho phép truy cập public** tất cả các tập tin đó lên trên S3 bucket mà bạn đã tạo, sử dụng câu lệnh sau :

```bash
aws s3 cp . s3://idevelop-lab5-<yourinitials> --recursive --region ap-northeast-1  --profile aws-lab-env --acl public-read
```

Các tập tin sẽ được sao chép vào S3 bucket của bạn.

12. Sau khi lệnh thực thi, bạn đã có thể xem lại trang web trong trình duyệt của mình. Mở trình duyệt web và duyệt đến vị trí sau (đảm bảo bạn cập nhật tên bucket để bao gồm tên viết tắt của bạn, để tên bucket khớp với tên bucket bạn đã tạo): 

```
http://idevelop-lab5-<yourinitials>.s3-website-ap-northeast-1.amazonaws.com/sqs-standard.html
```

13. Nếu tất cả đều ổn, bạn sẽ được chào đón bằng trang hiển thị sau trong trình duyệt: 

![Website](/images/2/5.png?width=90pc)

Để chắc chắn, hãy sử dụng chế độ xem nhà phát triển của trình duyệt để kiểm tra bảng điều khiển không có bất kỳ lỗi nào khi kết nối với các dịch vụ AWS. 

#### Thiết lập SQS Queue

Thiết lập môi trường bài thực hành đã cấu hình hàng đợi SQS được sử dụng trong bài thực hành này, sử dụng CloudFormation template. 

14. Mở **SQS console** trong trình duyệt. Bạn sẽ thấy các hàng đợi đã được cấu hình cho bạn. Các hàng đợi này sẽ được sử dụng trong quá trình kiểm tra hoạt động của hàng đợi SQS sau đây: 

![SQS](/images/2/6.png?width=90pc)

15. Bốn hàng đợi đã được thiết lập. Ba là hàng đợi tiêu chuẩn và một là hàng đợi FIFO. Đây là một đoạn mã của CloudFormation template đã thiết lập các tài nguyên này: 

```json
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
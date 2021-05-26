+++
title = "SNS Publisher tới nhiều SQS Subcribers"
date = 2021
weight = 6
chapter = false
pre = "<b>2.6. </b>"
+++

Trong [Tình huống 2.4](../4-sqs-pub-subs/), chúng ta đã thấy cách nhiều subscriber đọc từ cùng một hàng đợi SQS có thể 'giành lấy' thông tin của nhau, do đó không có subscriber nào nhìn thấy tất cả các thông tin trong hàng đợi. Đôi khi yêu cầu nhiều subscriber trong một hàng đợi nhận được tất cả các thông tin, vì vậy cần có một số cơ chế để gửi một thông tin cho nhiều hàng đợi. Giải pháp là sử dụng một phương thức trung gian để 'truyền tải' thông điệp đến nhiều subscriber. Trung gian trong trường hợp này là **Amazon Simple Notification Service (SNS)**. Amazon SNS cho phép một ứng dụng gửi một mục đích thông điệp duy nhất và gửi thông điệp đó đến một hoặc nhiều mục tiêu. (Các) mục tiêu của thông báo có thể là HTTP endpoint, thiết bị di động hoặc hàng đợi SQS - và trên thực tế là sự kết hợp giữa nhiều hoặc bất kỳ thành phần nào trong những thành phần đã liệt kê. Kiến trúc cho kịch bản này trông giống như sau:

![SNS](/images/2/snssqsarchitecture.png?width=50pc)

**Nội dung**
- [Cấu hình SNS fan-out cho nhiều hàng đợi SQS như thế nào?](#cấu-hình-sns-fan-out-cho-nhiều-hàng-đợi-sqs-như-thế-nào)
- [Làm thế nào các thông tin được gửi đến một SNS topic từ Javascript?](#làm-thế-nào-các-thông-tin-được-gửi-đến-một-sns-topic-từ-javascript)

1. Để bắt đầu demo này, hãy nhấp vào danh sách trên trang web và chọn **AWS SNS publisher to multiple SQS subscribers**.
2. Khi quá trình khởi tạo trang hoàn tất, hãy vẽ một hình dạng trên khung của publisher. Hãy để ý xem hình dạng được hiển thị như thế nào trên các khung bên phải của subscriber, với mỗi khung thể hiện giống hệt nhau và trình bày đầy đủ như khung của publisher? Điều này là do các điểm vẽ được gửi đến một Amazon SNS topic, từ đó gửi 'rẽ quạt' tới ba hàng đợi Amazon SQS, để mỗi hàng đợi nhận được tất cả các thông tin. Cuối cùng, cả ba subscriber sẽ hiển thị hình dạng đầy đủ, mặc dù họ sẽ nhận được thông tin điểm vẽ theo các thứ tự khác nhau và vẽ theo thứ tự mà chúng đã được nhận.

![SNS](/images/2/12.png?width=90pc)

Lưu ý rằng thông tins Received statistic đã nhận gấp 3 lần thông tins Sent statistic, được mong đợi nếu các thông tin được gửi được 'rẽ quạt' thành ba hàng đợi.

#### Cấu hình SNS fan-out cho nhiều hàng đợi SQS như thế nào?

Việc thêm một hoặc nhiều hàng đợi SQS làm mục tiêu cho các thông báo SNS rất đơn giản. Chúng ta hãy xem điều này ngay bây giờ.

3. Để xem lại cấu hình được sử dụng trong bài thực hành này, hãy mở **SNS console** trong trình duyệt.
4. Nhấp vào liên kết Chủ đề trên bảng điều hướng để hiển thị SNS topic **iDevelopthông tinDuplicator**:

![SNS](/images/2/13.png?width=90pc)

5. Nhấp vào arn để xem chi tiết các subscription cho SNS topic này: 

![SNS](/images/2/14.png?width=90pc)

Bạn sẽ thấy ba subscriber - một subscriber đại diện cho một trong ba hàng đợi SQS **arn:aws:sqs:us-west-2:277844687642:idevelop_drawqueue_XX* trong đó *XX** là từ ```1``` tới ```3```.

Khi một thông tin được gửi đến SNS topic, nó sẽ tự động được chuyển đến từng subscriber, trong trường hợp này là ba hàng đợi SQS. Khi trình duyệt gửi các điểm vẽ đến SNS topic, chúng cũng được chuyển đến ba hàng đợi SQS, mỗi hàng được triển khai trên trang web và các điểm được hiển thị trên khung của ba subscriber.

#### Làm thế nào các thông tin được gửi đến một SNS topic từ Javascript?

Đoạn mã Javascript này cho biết cách các thông tin được gửi đến SNS topic:

```js
var params = {
	thông tin: JSON.stringify(point),
	TopicArn: "<SNS Topic ARN>"
};

snsInstance.publish(params, function(err, data){});
```

**Bạn có thể tìm thêm thông tin về AWS SDK cho Javascript và SNS ở đâu?**

http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html

+++
title = "SQS Publisher tới SQS Subcriber"
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++

#### SQS Publisher tới SQS Subcriber
Trong trường hợp này, chúng ta sẽ sử dụng các hàng đợi này và xem xét một trường hợp đơn giản trong đó chúng ta có publisher đẩy các thông tin vào hàng đợi SQS và một subscriber đọc từ cùng một hàng đợi. Kiến trúc trông như thế này:
![SQS Publisher to SQS Subscriber](/images/3-messaging-options/3.2-sqs-pub-sub/architecture.png?featherlight=false&width=70pc)

Trang web sử dụng AWS SDK cho Javascript trong trình duyệt và xác thực ẩn danh với Amazon Cognito để lấy thông tin đăng nhập tạm thời, phạm vi mà nó có thể ký yêu cầu kết nối với dịch vụ Amazon SQS, để gửi tin và đăng ký vào cùng một hàng đợi SQS.

Trong bản demo này, publisher được thể hiện trên khung màu xám bên trái, subscriber ở bên phải. Sử dụng chuột của bạn, vẽ một hình dạng bằng cách nhấp xuống bằng nút chuột trái trong khi bạn di chuyển chuột để để lại một đường các pixel. Khi bạn vẽ, thông tin sẽ được gửi đến hàng đợi SQS chứa thông tin về tọa độ X & Y của chuột. Các thông tin này sẽ được nhận bởi trình duyệt đang lấy thông tin từ cùng một hàng đợi SQS và được hiển thị trên khung vẽ bên phải.
![SQS Publisher to SQS Subscriber](/images/3-messaging-options/3.2-sqs-pub-sub/sqs-pub-sub-001.png?featherlight=false&width=90pc)

Chú ý vì sao các điểm được vẽ không theo thứ tự ở phía bên tay phải. Điều này là do các thông tin nhận được qua SQS được nhận theo thứ tự không xác định. Đây là một đặc điểm của **standard SQS queue**.

Chú ý bảng thống kê ở phía publisher - nó hiển thị các SQS thông tin mà nó đã gửi và những thông tin đã gọi HTTP hoàn tất thành công. Khi bạn ngừng vẽ, hai con số này sẽ giống nhau sau một thời gian.

Ở bên phải, bạn sẽ thấy hiển thị subscriber. Khung màu xám cuối cùng sẽ phản chiếu bản vẽ trong khung của publisher. Sẽ mất một vài phút để tất cả các thông tin được nhận và hiển thị trên khung dành cho subscriber. Cuối cùng, thống kê trong bảng subscriber sẽ hiển thị số lượng thông tin nhận được giống như số liệu thống kê tin nhắn đã gửi trong bảng publisher.

Những gì bạn sẽ nhận thấy trong bản trình diễn này là hình dạng sẽ hiển thị các điểm vẽ trên bảng subscriber theo thứ tự khác với thứ tự chúng được vẽ. Mặc dù cuối cùng, mọi điểm vẽ sẽ được hiển thị.

#### Điều gì gây ra sự chậm trễ giữa gửi và nhận?
Do bản chất của mô hình phân luồng trong trình duyệt và sự xung đột trên luồng mạng thông qua trình duyệt, sẽ có một số độ trễ và xung đột giữa các bên gửi và nhận của phần demo. Amazon SQS có khả năng mở rộng và hiệu suất cao - nhưng bản demo này không được thiết kế để minh họa về hiệu suất. Nó được thiết kế để làm nổi bật việc tiếp nhận tin nhắn không theo thứ tự. Không có sự phối hợp giữa publisher và subscriber trong bản demo này và trình duyệt web không triển khai khả năng ‘tạo luồng’ để chạy hai tác vụ mô phỏng, vì vậy chúng xảy ra xung đột.

#### Làm cách nào để biết được độ trễ giữa gửi và nhận?
Nếu bạn nhấp vào nút **Clear drawing** một vài lần và xem thống kê **Average Latency** trên bảng subscriber, bạn sẽ biết được độ trễ đầu cuối của bản demo. Khi bạn nhấp vào nút **Clear drawing**, một thông tin sẽ được gửi đến hàng đợi SQS. Điều này sẽ làm giảm sự xung đột và tác động của việc phân luồng trong trình duyệt về thời gian của thông tin pub/sub.

#### Mã Javascript để lấy thông tin đăng nhập từ Amazon Cognito
Để các demo trong bài thực hành này có thể kết nối và truy vấn môi trường AWS của bạn (ví dụ: để gửi thông tin đến hàng đợi), mã Javascript đang chạy trong trình duyệt cần có thông tin đăng nhập. Bạn không bao giờ được nhúng Access Key/Secret Access Key vào các ứng dụng của mình, vì vậy cần phải có một cơ chế khác để trang web lấy thông tin đăng nhập tự động. Amazon Cognito cung cấp cơ chế cung cấp thông tin xác thực tạm thời cho các môi trường không đáng tin cậy, chẳng hạn như các trang web. Trong những demo này, chúng ta sẽ sử dụng chế độ truy cập ‘unauthenticated’ hoặc ‘anonymous’ với Amazon Cognito. Trong chế độ này, trang web có thể lấy thông tin xác thực tạm thời mà người dùng không cần phải cung cấp thông tin xác thực.

Lấy thông tin đăng nhập chỉ mất một vài dòng mã. Bài thực hành đã thiết lập cấu hình Cognito Identity Pool cho bạn và bạn đã cập nhật tập tin aws-config.js trước đó, thêm giá trị Cognito IdentityPoolId cung cấp cho mã Javascript.

Đây là một đoạn mã lấy thông tin xác thực, được lấy từ tập tin **Messaging/scripts/cognito-bootstrap.js** trong tập tin zip:
```
var params = {
	IdentityPoolId : config.Cognito.IdentityPoolId
};

AWS.config.region       = config.Region;
AWS.config.credentials  = new AWS.CognitoIdentityCredentials(params);

AWS.config.credentials.get(function(err)
{
	if (!err)
	{
		console.log("Cognito Identity Id: " + AWS.config.credentials.identityId);
		connectedCallback(configuration);
	}
	else
	{
		console.log("Cognito Error: " + err );
	}
});
```

Bạn chỉ cần cung cấp Cognito Identity Pool Id cho lệnh gọi tới **AWS.config.credentials.get** từ SDK Javascript và khi lệnh gọi thành công, SDK sẽ tự động sử dụng Access Key/Secret Access Key trả về từ lệnh gọi. Kể từ thời điểm này, mọi lệnh gọi tiếp theo thông qua SDK Javascript tới các AWS endpoint đều được ký bằng các thông tin đăng nhập đó.

#### Làm thế nào các tin nhắn được gửi vào hàng đợi từ javascript?
Đoạn mã Javascript này cho biết cách thông tin được gửi vào hàng đợi SQS:
```
var params = {
  MessageBody: JSON.stringify(point),
  QueueUrl: "<The Queue Url>",
};

sqsInstance.sendMessage(params, function (err, data) {});
```

Làm thế nào các thông tin nhận được từ hàng đợi từ Javascript?

Đoạn mã Javascript này cho biết cách nhận thông tin từ hàng đợi SQS:
```
var params = {
  MaxNumberOfMessages: 10,
  QueueUrl: "<The Queue Url>",
  VisibilityTimeout: 20,
  WaitTimeSeconds: 20,
};

sqsInstance.receiveMessage(params, function (err, data) {});
```

#### Bạn có thể tìm thêm thông tin về AWS SDK cho Javascript và SQS ở đâu?

http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html

#### Kiểm tra các thông tin trong hàng đợi SQS

Sử dụng AWS console, có thể xem các tin nhắn đã được gửi vào hàng đợi SQS nhưng subscriber chưa nhận được. Đây là một tính năng tiện dụng trong quá trình thiết kế và gỡ lỗi một ứng dụng.

1. Truy cập [**AWS SQS console**](https://console.aws.amazon.com/sqs/home).
* Chọn **idevelop_drawqueue_1**
* Click **Send and recieve messages**
![SQS Publisher to SQS Subscriber](/images/3-messaging-options/3.2-sqs-pub-sub/sqs-pub-sub-002.png?featherlight=false&width=90pc)
2. Click **Start Polling for messages**, Trong một tab trình duyệt khác, vẽ thêm điểm trên khung của publisher, sau đó chuyển trở lại.
![SQS Publisher to SQS Subscriber](/images/3-messaging-options/3.2-sqs-pub-sub/sqs-pub-sub-003.png?featherlight=false&width=90pc)
* Click id của message để xem thông tin chi tiết
![SQS Publisher to SQS Subscriber](/images/3-messaging-options/3.2-sqs-pub-sub/sqs-pub-sub-004.png?featherlight=false&width=90pc)

Việc xem thông tin theo cách này không làm mất chúng, chúng vẫn có sẵn cho subscriber xử lý. Nhưng nó là một công cụ hữu ích, để kiểm tra xem các thông tin có chạy qua các hàng đợi như mong đợi trong giai đoạn phát triển hay không.





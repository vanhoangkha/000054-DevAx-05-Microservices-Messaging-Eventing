+++
title = "Microservices cho Truyền tin & Sự kiện"
date = 2021
weight = 1
chapter = false
+++

# Microservices Messaging & Eventing

Trong bài thực hành này, bạn sẽ khám phá các tùy chọn khác nhau mà nền tảng AWS có để truyền tin. Truyền tin là một khái niệm quan trọng cần hiểu khi thiết kế các kiến trúc có thể mở rộng, tách rời, làm cho chúng hoạt động được trước sự cố. Xây dựng ứng dụng từ các thành phần riêng lẻ thực hiện một chức năng riêng biệt giúp cải thiện khả năng mở rộng và độ tin cậy, đồng thời là thiết kế thực tiễn tốt nhất cho các ứng dụng hiện đại. AWS có một số giải pháp truyền tin, từ hàng đợi đến luồng trực tuyến và chúng ta sẽ tìm hiểu về cách hoạt động của từng giải pháp khi chúng ta đi qua bài thực hành.

#### Amazon SQS
Amazon Simple Queue Service (SQS) là dịch vụ hàng đợi tin nhắn được quản lý hoàn toàn, cho phép bạn ngắt liên kết và điều chỉnh quy mô của các vi dịch vụ, hệ thống phân tán và ứng dụng serverless. SQS giúp loại bỏ sự phức tạp và công sức tổng thể liên quan đến việc quản lý và vận hành phần mềm trung gian định hướng tin nhắn và cho phép nhà phát triển tập trung vào công việc khác. Khi dùng SQS, bạn có thể gửi, lưu trữ và nhận tin nhắn giữa các thành phần của phần mềm ở bất cứ khối lượng nào, mà không làm mất tin nhắn hay bắt các dịch vụ khác phải luôn luôn trong trạng thái khả dụng. Bắt đầu với SQS trong vài phút bằng cách sử dụng bảng điều khiển AWS, Giao diện dòng lệnh hoặc SDK mà bạn chọn và ba lệnh đơn giản.

SQS cung cấp hai loại hàng đợi tin nhắn. Hàng đợi tiêu chuẩn sẽ đem lại lưu lượng tối đa, sắp xếp thứ tự nỗ lực tối đa và phát chuyển ít nhất một lần. Hàng đợi FIFO SQS được thiết kế để đảm bảo rằng tin nhắn sẽ được xử lý đúng một lần, theo đúng thứ tự được gửi.

#### Amazon SNS
Amazon Simple Notification Service (Amazon SNS) là dịch vụ nhắn tin được quản lý toàn phần dành cho cả phương thức liên lạc từ hệ thống đến hệ thống (A2A) cũng như từ ứng dụng đến người dùng (A2P). Chức năng pub/sub A2A cung cấp các chủ đề cho phương thức nhắn tin thông lượng cao, theo mô hình đẩy, nhiều-nhiều giữa các hệ thống phân tán, vi dịch vụ và các ứng dụng phi máy chủ theo sự kiện. Bằng cách dùng các chủ đề của Amazon SNS, các hệ thống phát hành của bạn có thể phân phối tin nhắn đến một số lượng lớn hệ thống đăng ký bao gồm hàng chờ Amazon SQS, các hàm AWS Lambda và điểm cuối HTTPS, cho quá trình xử lý song song, cũng như Amazon Kinesis Data Firehose. Chức năng A2P cho phép bạn gửi tin nhắn tới người dùng theo quy mô lớn qua SMS, thông báo đẩy trên di động và email.

#### Amazon Kinesis
Amazon Kinesis giúp cho việc thu thập, xử lý và phân tích dữ liệu thời gian thực, theo luồng dễ dàng hơn để bạn có thể có được thông tin chuyên sâu kịp thời và phản ứng nhanh với thông tin mới. Amazon Kinesis cung cấp những khả năng chủ chốt để xử lý dữ liệu tại bất kỳ quy mô nào một cách tiết kiệm, cùng với việc linh hoạt lựa chọn những công cụ phù hợp với yêu cầu từ ứng dụng của bạn. 

#### Amazon IoT
AWS IoT là một nền tảng đám mây được quản lý cho phép các thiết bị được kết nối tương tác dễ dàng và an toàn với các ứng dụng đám mây và các thiết bị khác. AWS IoT có thể hỗ trợ hàng tỷ thiết bị và hàng nghìn tỷ thông báo, đồng thời có thể xử lý và định tuyến các thông báo đó đến các điểm cuối AWS và đến các thiết bị khác một cách đáng tin cậy và an toàn. AWS IoT cũng có thể được sử dụng bởi các ứng dụng của bạn khi bạn cần kênh pub/sub tạm thời để truyền tin nhanh, nhẹ giữa các thành phần.

#### Các chủ đề được đề cập
Vào cuối bài thực hành này, bạn sẽ có thể:
- Hiểu các hoạt động của các phương thức truyền tin khác nhau trên nền tảng AWS
- Giải thích sự khác biệt giữa hàng chờ và luồng dữ liệu
- Sử dụng các phương thức truyền tin trên AWS từ trình duyệt web và lập trình bằng Java SDK
- Hiểu cách đọc dữ liệu từ Amazon Kinesis stream bằng hàm AWS Lambda
- Tạo kênh giao tiếp pub/sub giữa hai EC2 instance, thông qua Amazon Kinesis stream, sử dụng AWS SDK cho Java
- Tìm hiểu cách tạo kho lưu trữ git trong AWS CodeCommit và truy cập kho lưu trữ trong môi trường phát triển và EC2 instance

#### Điều kiện tiên quyết về Kiến thức Kỹ thuật
Để hoàn thành thành công bài thực hành này, bạn nên làm quen với điều hướng cơ bản của AWS Management Console và có thể thoải mái chỉnh sửa tập lệnh bằng trình chỉnh sửa văn bản.

#### Nội dung

1. [Các bước chuẩn bị](1-prerequisites/)
2. [Lựa chọn cách truyền tin](2-messaging-options/)
3. [Truyền thông tin với Kinesis](3-message-streaming/)
4. [Bài tập nâng cao](4-challenge/)
5. [Kết luận](5-conclusion/)
6. [Tài liệu tham khảo](6-resources/)
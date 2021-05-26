+++
title = "Truyền thông tin với Kinesis"
date = 2021
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

Khi chúng ta sử dụng Kinesis trong trình duyệt để gửi thông tin, thông lượng tới Kinesis bị hạn chế bởi mô hình phân luồng trong trình duyệt và thực tế là chúng ta đang viết từng bản ghi vào Kinesis stream. Phần demo được thiết kế để minh họa một trường hợp sử dụng đơn giản và cách gắn Lambda consumer vào Kinesis stream, không phải minh họa khả năng mở rộng của dịch vụ. Trong bài tập này, chúng ta sẽ xem xét các khía cạnh về khả năng mở rộng và thông lượng.

Bạn có thể mở rộng Kinesis bằng cách sử dụng các shard. Mỗi shard có thể hỗ trợ tối đa 5 giao dịch mỗi giây cho việc đọc, lên đến tổng tốc độ đọc dữ liệu tối đa là 2 MB mỗi giây. Mỗi shard cũng có thể hỗ trợ tối đa 1.000 bản ghi mỗi giây cho việc ghi, lên đến tổng tốc độ ghi dữ liệu tối đa là 1 MB mỗi giây. Giới hạn ghi này áp dụng cho các hoạt động như PutRecord và PutRecords.

Bạn có thể gửi tối đa 500 bản ghi trong một lệnh gọi PutRecords và nhận tới 10.000 bản ghi trong một lệnh gọi GetRecords.

**Kinesis Client Library** xử lý các cơ chế cơ bản của việc sử dụng Kinesis, giữ trạng thái trong Amazon DynamoDB table và quản lý sự phức tạp của các shard, vòng lặp shard, v.v. Trong bài tập này, chúng ta sẽ chạy hai chương trình Java, một chương trình tạo ra bản ghi và một chương trình sử dụng bản ghi từ Kinesis stream. Consumer sẽ ghi các bản ghi mà họ nhận được vào một chỉ mục miền **Amazon ElasticSearch** để chúng ta có thể chạy các truy vấn trên dữ liệu. Consumer sử dụng Kinesis Client Library để giảm thời gian phát triển.

Chúng ta cũng sẽ tạo hai kho AWS CodeCommit mới để chia sẻ mã giữa môi trường phát triển của chúng ta và EC2 instance sẽ chạy mã trong môi trường thử nghiệm của chúng ta.

#### Nội dung
1. [Các bước chuẩn bị](../3-message-streaming/1-prequistite/)
2. [Tạo Kinesis Producer](../3-message-streaming/2-kinesis-producer/)
3. [Gửi các bản ghi từ Producer đến Consumer](../3-message-streaming/3-send-records/)
4. [Kiểm tra bản ghi trong ElasticSearch](../3-message-streaming/4-verify-elasticsearch/)
5. [Khám phá và Đồ thị hóa dữ liệu trong Kibana](../3-message-streaming/5-kibana/)

+++
title = "Bài tập nâng cao"
date = 2021
weight = 4
chapter = false
pre = "<b>4. </b>"
+++

Nếu bạn có thời gian nhiều hơn, hãy thử thực hiện một số các bước bên dưới nhằm củng cố thêm kiến thức mà bạn đã học ở module này. Nếu bạn không có thời gian để triển khai, bạn có thể suy nghĩ về hướng giải quyết và thảo luận với mọi người.
- Điều chỉnh Lambda function đã sử dụng trong Kinesis-To-SQS để điều chỉnh dữ liệu theo một cách khác như là thay đổi tọa độ X thay vì tọa độ Y như trong bài.
- Điều chỉnh Lambda function đã sử dụng trong Kinesis-To-SQS nhằm gửi dữ liệu nhận được từ Kinesis stream đến các SNS topic sử dụng mô hình fan-out, như vậy bạn sẽ thấy được nhiều comsumer sẽ nhận được thông tin từ stream.
- Thêm một Lambda function để Kinesis stream thực hiện xử lý stream một cách song song.
- Thử nghiệm với các tính năng dashboard của Kibana để tạo biểu đồ nêu bật thông tin về dữ liệu kinh doanh được gửi giữa hai EC2 instance.
+++
title = "Kinesis Publisher tới SQS Subcriber"
date = 2021
weight = 7
chapter = false
pre = "<b>2.7. </b>"
+++

Amazon Kinesis giúp bạn dễ dàng thu thập, xử lý và phân tích luồng dữ liệu truyền liên tục, real-time để bạn có thể nhận được thông tin chi tiết kịp thời và phản ứng nhanh với thông tin mới. Trong trường hợp này, trình duyệt sẽ gửi các điểm vẽ tới Kinesis stream. Thông thường, chúng ta sẽ tạo một Kinesis consumer client và đọc trực tiếp các sự kiện từ stream và xử lý chúng, và chúng ta sẽ thực hiện điều này trong kịch bản tiếp theo. Đối với tình huống này, giả sử chúng ta muốn các thông tin được gửi đến hàng đợi SQS giống như chúng ta đã làm trong các tình huống trước đây.

Tùy thuộc vào việc bạn chọn sử dụng **Eclipse** hay **AWS Cloud9**, hãy chọn hướng dẫn thực hành thích hợp bên dưới:

**Nội dung**
1. [Lựa chọn A: Sử dụng Eclipse](a/)
2. [Lựa chọn B: Sử dụng AWS Cloud9](b/)
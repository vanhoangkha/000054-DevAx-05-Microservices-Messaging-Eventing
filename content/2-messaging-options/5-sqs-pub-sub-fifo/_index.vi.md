+++
title = "SQS Publisher tới SQS Subscriber với FIFO"
date = 2021
weight = 5
chapter = false
pre = "<b>2.5. </b>"
+++

Trong một số ứng dụng, tính chất không xác định thứ tự các thông tin nhận được trong hàng đợi SQS có thể có vấn đề. Ví dụ: nếu hàng đợi chứa các hoạt động CRUD, chúng cần được subscriber thực hiện theo thứ tự mà chúng đã được gửi đi, vì các giả định về trạng thái của dữ liệu hiện tại quyết định các hoạt động tiếp theo có thể được thực hiện trên dữ liệu. Trong trường hợp này, chúng ta xem xét yêu cầu này, sử dụng hàng đợi SQS First-In-First-Out (FIFO).

1. Để bắt đầu bản demo này, hãy nhấp vào danh sách trên trang web và chọn **AWS SQS (FIFO) publisher to SQS subscriber**
2. Khi quá trình khởi tạo trang hoàn tất, hãy vẽ một hình dạng trong khung của publisher ở bên trái và quan sát hoạt động của khung dành cho subscriber ở bên phải. Khi bạn vẽ, thông tin sẽ được gửi đến hàng đợi SQS FIFO chứa thông tin về tọa độ X & Y của chuột. Các thông tin này sẽ được nhận bởi trình duyệt đang lấy dữ liệu cùng hàng đợi SQS - một subscriber duy nhất. Các điểm vẽ sẽ được hiển thị theo **cùng một thứ tự** mà bạn đã vẽ chúng, và do đó, **cùng một thứ tự** chúng đã được xuất bản vào hàng đợi SQS FIFO. Hoạt động của FIFO đã bảo toàn thứ tự thông tin mà bỏ qua thông lượng và độ trễ. Bạn có thể nhận thấy có thêm một khoảng thời gian trễ trước khi bắt đầu nhận được thông tin. Một lần nữa, bản demo này không được thiết kế để làm nổi bật hiệu suất, và do đó, sự chậm trễ này là do xung đột về luồng của trình duyệt và mạng.

![SQS](/images/2/11.png?width=90pc)

Để biết thêm thông tin về hàng đợi SQS FIFO, hãy xem tài liệu AWS tại http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html

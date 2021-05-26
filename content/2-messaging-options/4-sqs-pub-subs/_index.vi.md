+++
title = "SQS Publisher tới nhiều SQS Subscriber"
date = 2021
weight = 4
chapter = false
pre = "<b>2.4. </b>"
+++

Trong trường hợp này, chúng ta xem xét hành vi khi nhiều subscriber đang nhận dữ liệu cùng một hàng đợi SQS cho các thông tin. Trong trường hợp này, các subscriber sẽ cạnh tranh về thông tin và một số thông tin sẽ được xác nhận bởi một subscriber, một số thông tin khác sẽ được xác nhận bởi những subscriber khác. Nếu dữ liệu đang được gửi được coi là một luồng liên tục về các dữ liệu liên quan, các subscriber khác nhau sẽ cần phải phối hợp để hiểu được thông tin nếu chúng là một phần của một chuỗi liên tục. Nếu không có sự phối hợp, thứ tự nhận thông tin và subscriber nhận từng thông tin là không xác định. Mô hình này phù hợp khi không cần sự phối hợp giữa các subscriber để xử lý thành công các thông tin trong hàng đợi và khi thứ tự thông tin được sử dụng bởi từng subscriber riêng lẻ và thực tế là bất kỳ subscriber cụ thể nào đề có thể bỏ lỡ thông tin, điều này không quan trọng đối với thiết kế của ứng dụng. Kiến trúc sẽ như thế này:

![SQS](/images/2/sqspubmultisub-architecture.png?width=50pc)

1. Để bắt đầu demo, hãy nhấp vào danh sách trên trang web và chọn **AWS SQS (standard) publisher to multiple SQS subscribers**.
2. Khi quá trình khởi tạo trang hoàn tất, hãy vẽ một hình dạng trong khung của publisher ở bên trái và quan sát hoạt động của ba khung của subscriber ở bên phải. Khi bạn vẽ, thông tin sẽ được gửi đến hàng đợi SQS chứa thông tin về tọa độ X & Y của chuột. Các thông tin này sẽ được nhận bởi trình duyệt đang nhận dữ liệu cùng một hàng đợi SQS với 3 subscriber khác nhau, mỗi subscriber hiển thị các sự kiện chúng nhận được trên khung của riêng chúng.

![SQS](/images/2/10.png?width=90pc)

*Lưu ý rằng các điểm được vẽ không theo thứ tự ở phía bên phải và không có khung vẽ nào có bản vẽ hoàn chỉnh?*

Các điểm được vẽ ra không theo thứ tự vì thông tin nhận được qua hàng đợi SQS được nhận theo thứ tự không xác định. Đây là một đặc điểm của hàng đợi standard SQS. Và vì mỗi khung đang nhận dữ liệu cùng một hàng đợi SQS, chúng đang cạnh tranh cho các thông tin trong hàng đợi, và sẽ tự ý nhận các điểm và hiển thị chúng, giành lấy chúng từ các khung khác.

Bất kể khung subscriber nào nhận được thông tin điểm vẽ, tất cả các thông tin đã gửi sẽ được nhận - bạn có thể xác nhận điều này bằng cách so sánh bộ đếm **thông tins Sent** với bộ đếm **thông tins Received** trong bảng thống kê của publisher - chúng cuối cùng sẽ bằng nhau.
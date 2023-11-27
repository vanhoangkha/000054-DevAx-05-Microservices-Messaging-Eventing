+++
title = "MQTT Publisher tới MQTT Subcriber"
date = 2020
weight = 7
chapter = false
pre = "<b>3.7. </b>"
+++

#### MQTT Publisher tới MQTT Subcriber

Hành động gửi thông tin cuối cùng cần khám phá là **Lightweight MQTT messaging**, sử dụng **AWS IoT** như một kênh pub/sub tạm thời. Trong trường hợp này, các sự kiện vẽ được gửi cho một MQTT topic tùy chỉnh. Cùng một topic tùy chỉnh này được đăng ký bởi khung nhận và điểm được hiển thị. Độ trễ giữa publisher và subscriber là rất thấp, nhưng lưu ý rằng tất cả các message được gửi trên kênh là tạm thời - nếu subscriber không trong trạng thái “listening” khi publisher gửi tin nhắn, tin nhắn sẽ bị mất vĩnh viễn.

Kiến trúc cho kịch bản này trông giống như sau:
![MQTT Publisher to MQTT Subscriber](/images/3-messaging-options/3.7-mqtt-pub-sub/iotpubsubarchitecture.png?featherlight=false&width=70pc)

1. Để bắt đầu demo này, hãy chọn vào danh sách trên trang web và chọn **AWS IoT MQTT publisher to MQTT subscriber**. Khi trang khởi chạy, hãy vẽ một hình trên khung MQTT Publisher. Khung vẽ của MQTT Subscriber sẽ cập nhật gần như ngay lập tức đồng bộ với bản vẽ trên publisher.
![MQTT Publisher to MQTT Subscriber](/images/3-messaging-options/3.7-mqtt-pub-sub/mqtt-pub-sub-001.png?featherlight=false&width=90pc)

Để biết về độ trễ, hãy chọn vào **Clear drawing** một vài lần và xem **Average Latency statistic**.

Mở một cửa sổ trình duyệt khác và duyệt đến trang web demo và sắp xếp các cửa sổ để bạn có thể xem cả hai trang cùng một lúc. Chọn **AWS IoT MQTT publisher to MQTT subscriber slave** từ danh sách trên trình duyệt thứ hai. Bây giờ, hãy vẽ một hình trên MQTT Publisher. Lưu ý rằng hình xuất hiện trên khung MQTT Subscriber trên cả hai trang cùng một lúc. Các tin nhắn MQTT tự động gửi ‘rẽ quạt’ đến nhiều subscriber. Bạn không cần cung cấp trước các trang subscriber - chúng được đăng ký động vào cùng một kênh khi chúng kết nối. Không giống như SQS, nơi bạn cần cung cấp hàng đợi SQS trước.

Để chứng minh điều này, hãy yêu cầu một trong những đồng nghiệp của bạn cũng duyệt đến trang web của bạn và chọn trang slave. Các bức vẽ của bạn trên khung MQTT Publisher sẽ xuất hiện trên tất cả các khung của MQTT Subscriber khi bạn vẽ.

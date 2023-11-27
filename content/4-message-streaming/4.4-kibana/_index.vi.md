+++
title = "Khám phá Kibana "
date = 2020
weight = 4
chapter = false
pre = "<b>4.4. </b>"
+++
#### Khám phá và Đồ thị hóa dữ liệu trong Kibana

Amazon ElasticSearch đi kèm với plugin Kibana được cài đặt để hỗ trợ bạn trực quan hóa và phân tích dữ liệu. Do cách thức hoạt động của mô hình bảo mật, tất cả các lệnh gọi vào ElasticSearch cluster, bao gồm cả kết nối với Kibana, phải được ký bằng thông tin xác thực. Để chúng ta có thể kết nối với Kibana, chúng ta cần phải ký vào yêu cầu mà trình duyệt không thể thực hiện hoặc chúng ta cần cho phép thông qua địa chỉ IP. Hãy thực hiện phần sau và thêm IP của chúng ta vào chính sách bảo mật cluster.

1. Truy cập [**Amazon ElasticSearch Service console**](https://console.aws.amazon.com/esv3/home).
* Click **Domains**
* Click **idevelop**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-001.png?featherlight=false&width=90pc)
2. Trong trang **idevelop**
* Click tab **Security Configuaration**
* Click **Edit**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-002.png?featherlight=false&width=90pc)
3. Trong phần **Access policy**
* Tìm phần **Condition** và **IpAddress** nhập địa chỉ IP của bạn
{{% notice tip %}} 
Bạn có thể sử dụng https://checkip.amazonaws.com/ để xác định IP của mình hoặc sử dụng một số công cụ khác như ```curl ifconfig.me```
{{% /notice %}}
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-003.png?featherlight=false&width=90pc)
* Kéo xuống dưới, click **Save changes**
4. Click link trong phần **Kibana URL**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-005.png?featherlight=false&width=90pc)
5. Nhiệm vụ đầu tiên của bạn là thiết lập một index pattern mặc định. Trong trường bên dưới **Index name or pattern**, hãy thay thế nội dung bằng ```*```
* Click **Create**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-006.png?featherlight=false&width=90pc)
6. Bạn có thể không thấy bất kỳ bản ghi nào được hiển thị. Tại thời điểm này, Kibana đang mặc định chỉ hiển thị cho bạn các bản ghi có giá trị timestamp trong phạm vi hiện tại trừ 15 phút trước. Ứng dụng của chúng ta tạo ra các sự kiện bán hàng với dấu thời gian bắt đầu từ năm 2015 và tiến về phía trước với mức tăng ngẫu nhiên. Do đó, bạn có thể không thấy bất kỳ bản ghi nào cho khung thời gian kéo dài 15 phút qua. Mở rộng khung thời gian bằng cách nhấp vào liên kết **Last 15 minutes** ở trên cùng bên phải của trang và trong danh sách xuất hiện, hãy chọn **Last 5 years**. Bây giờ bạn sẽ thấy nhiều dữ liệu hơn.
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-007.png?featherlight=false&width=90pc)
7. Bấm vào một trong các bản ghi trong danh sách để mở rộng nó. Các trường trong dữ liệu liên quan đến bản ghi giao dịch của chúng tôi cho doanh số bán hàng giả tưởng - productLine, product, totalCost, v.v.
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-008.png?featherlight=false&width=90pc)

Nếu bạn tham khảo lại mã Java trong Eclipse IDE, trong cả ứng dụng Producer và Consumer, có một class được gọi là SaleItemTransaction có các trường tương tự trong đó. Class này được sử dụng để serialise và deserialise dữ liệu bán hàng vào và ra khỏi Kinesis stream và xa hơn nữa, vào chỉ mục ElasticSearch như bạn có thể thấy ở đây.

#### Đồ thị hóa dữ liệu trong Kibana
Trong phần này, chúng ta sẽ đồ thị hóa và phân tích dữ liệu bán hàng bằng Kibana.
1. Click **Visualize**
* Click **Pie chart**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-009.png?featherlight=false&width=90pc)
2. Click **From a new search**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-010.png?featherlight=false&width=90pc)
3. Ban đầu, bạn sẽ thấy một biểu đồ hình tròn là một vòng tròn đầy đủ chỉ được tạo thành từ một lát cắt. Chúng ta cần lọc dữ liệu của mình.
4. Click **Split Slices**
* Trong mục **Aggregation**, chọn **Terms**
* Trong mục **Field**, chọn **productType**
* Trong mục **Size**, nhập ```5```
* Click **Play icon**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-011.png?featherlight=false&width=90pc)
Tập dữ liệu của bạn sẽ khác (do các giao dịch và chi tiết của chúng được chọn ngẫu nhiên) nhưng bạn sẽ thấy một kết quả tương tự như được hiển thị ở đây. Trong ví dụ này, **eyewear** là sản phẩm di chuyển hàng đầu trong khoảng thời gian đã chọn (5 năm qua).
5. Click **Add sub-buckets**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-012.png?featherlight=false&width=90pc)
6. Click **Split Slices**
* Trong mục **Sub aggregation**, chọn **Terms**
* Trong mục **Field**, chọn **retailerType**
* Trong mục **Size**, nhập ```5```
* Click **Play icon**
![Discover & Visualise The Data In Kibana](/images/4-message-streaming/4.4-kibana/kibana-013.png?featherlight=false&width=90pc)

Bạn có thể thấy, Kibana là một công cụ rất mạnh cho phép bạn phân tích dữ liệu. Dành một chút thời gian để thử nghiệm với các loại đồ thị, trường và liên kết. Bạn có thể lưu lại các đồ thị mà bạn đã tạo và sau đó thêm chúng vào Dashboard sử dụng công vụ và biểu tượng trên trang Kibana.

Nếu bạn cần xóa các bản ghi, mapping và index từ miền ElasticSearch, bạn có thể chạy câu lệnh sau từ cửa sổ lệnh:
```
curl -XDELETE https://<ElasticsearchEndpoint>/transactions -k
```
Hãy thay thế giá trị **ElasticsearchEndpoint** từ tab **Cloudformation - Outputs**.

Bạn sẽ phải cần tạo lại các mapping, bằng cách chạy lại lệnh mapping mà bạn đã thực hiện ở bước trước. Bạn có thể thêm lại tập dữ liệu của bạn bằng cách chạy lại ứng dựng Consumer và Producer.
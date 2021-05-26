+++
title = "Khám phá Kibana"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5. </b>"
+++

**Khám phá và Đồ thị hóa dữ liệu trong Kibana**

Amazon ElasticSearch đi kèm với plugin Kibana được cài đặt để hỗ trợ bạn trực quan hóa và phân tích dữ liệu. Do cách thức hoạt động của mô hình bảo mật, tất cả các lệnh gọi vào ElasticSearch cluster, bao gồm cả kết nối với Kibana, phải được ký bằng thông tin xác thực. Để chúng ta có thể kết nối với Kibana, chúng ta cần phải ký vào yêu cầu mà trình duyệt không thể thực hiện hoặc chúng ta cần cho phép thông qua địa chỉ IP. Hãy thực hiện phần sau và thêm IP của chúng ta vào chính sách bảo mật cluster.

1. Đi đến **Amazon ElasticSearch Cluster** trong **AWS Console**.

2. Sau khi đã chọn cluster, chọn **Actions** và **Modify access policy**.

3. Bạn sẽ thấy một chuỗi JSON đã được thiết lập cho bạn. Tìm phần **Condition** và **IpAddress** Sau đó thêm vào địa chỉ IP của bạn (**X.X.X.X** thể hiện thông tin trước đó và bạn có thể bỏ qua):

```json
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": [
            "X.X.X.X",
            "<ADD YOUR PUBLIC IP OR RANGE>"
          ]
        }
      }
```

{{% notice tip %}}
Bạn có thể sử dụng https://checkip.amazonaws.com/ để xác định IP của mình hoặc sử dụng một số công cụ khác như ```curl ifconfig.me```
{{% /notice %}}

4. Sau khi bạn thay đổi Chính sách, bạn có thể nhấp vào liên kết Kibana trong tab tổng quan. 

5. Bạn sẽ thấy trang tải Kibana trong khi mọi thứ được thiết lập - sẽ chỉ mất một chút thời gian.

![ElasticSearch](/images/3/14.png?width=90pc)

6. Nhiệm vụ đầu tiên của bạn là thiết lập một index pattern mặc định. Trong trường bên dưới **Index name or pattern**, hãy thay thế nội dung bằng dấu hoa thị (*) như minh họa:

![ElasticSearch](/images/3/15.png?width=90pc)

Sau đó, Kibana sẽ tự động tìm trường **timestamp** trong chỉ mục giao dịch của chúng ta. Nhấp vào **Create** để xác nhận.

7. Click **Discover** in the top menu tab. You may not see any records rendered. At this point, Kibana is defaulting to only show you records that have a *timestamp* value in the range now minus 15 minutes ago. Our application generates sales events with timestamps starting in 2015 and advancing forward at random increments. Therefore, you may not see any records for the timeframe that spans the *Last 15 minutes*. Widen the timeframe by clicking on the **Last 15 minutes** link at the top right of the page, and in the list that appears, choose **Last 5 years**
Nhấp vào **Discover** trong menu trên cùng. Bạn có thể không thấy bất kỳ bản ghi nào được hiển thị. Tại thời điểm này, Kibana đang mặc định chỉ hiển thị cho bạn các bản ghi có giá trị *timestamp* trong phạm vi hiện tại trừ 15 phút trước. Ứng dụng của chúng ta tạo ra các sự kiện bán hàng với dấu thời gian bắt đầu từ năm 2015 và tiến về phía trước với mức tăng ngẫu nhiên. Do đó, bạn có thể không thấy bất kỳ bản ghi nào cho khung thời gian kéo dài *15 phút qua*. Mở rộng khung thời gian bằng cách nhấp vào liên kết **Last 15 minutes** ở trên cùng bên phải của trang và trong danh sách xuất hiện, hãy chọn **Last 5 years**

! [ElasticSearch] (/ images / 3 / 16.png? Width = 90pc)

    Bây giờ bạn sẽ thấy nhiều dữ liệu hơn!

8. Bấm vào một trong các bản ghi trong danh sách để mở rộng nó. Các trường trong dữ liệu liên quan đến bản ghi giao dịch của chúng tôi cho doanh số bán hàng giả tưởng - productLine, product, totalCost, v.v.

![ElasticSearch](/images/3/17.png?width=90pc)

9. Nếu bạn tham khảo lại mã Java trong Eclipse IDE, trong cả ứng dụng Producer và Consumer, có một class được gọi là ***SaleItemTransaction*** có các trường tương tự trong đó. Class này được sử dụng để serialise và deserialise dữ liệu bán hàng vào và ra khỏi Kinesis stream và xa hơn nữa, vào chỉ mục ElasticSearch như bạn có thể thấy ở đây.

#### Đồ thị hóa dữ liệu trong Kibana
Trong bước này, chúng ta sẽ đồ thị hóa và phân tích dữ liệu bán hàng bằng Kibana.

10. Chọn **Visualize** ở thanh menu.
11. Chọn **Pie chart** từ danh sách lựa chọn loại đồ thị.

![ElasticSearch](/images/3/18.png?width=90pc)

12. Ở bước 2, chọn **From a new search**

13. Ban đầu, bạn sẽ thấy một biểu đồ hình tròn là một vòng tròn đầy đủ chỉ được tạo thành từ một lát cắt. Chúng ta cần lọc dữ liệu của mình. Nhấp chuột **Split Slices**.

14. Ở **Aggregation** chọn ```Terms```

15. Ở **Field** chọn ```productType```

16. Ở **Size** nhập ```5```

17. Chọn biểu tượng **Play** ở trên cùng bảng điều khiển bên trái để vẽ lại đồ thị:

![ElasticSearch](/images/3/19.png?width=90pc)
 
  Tập dữ liệu của bạn sẽ khác (do các giao dịch và chi tiết của chúng được chọn ngẫu nhiên) nhưng bạn sẽ thấy một kết quả tương tự như được hiển thị ở đây. Trong ví dụ này, **Eyewear** là sản phẩm di chuyển hàng đầu trong khoảng thời gian đã chọn (5 năm qua).

18. Chọn **Add sub-buckets**
19. Chọn **Split slices**
20. Ở **Sub aggregation** chọn ```Terms```
21. Ở **Field** chọn ```retailerType```
22. Chọn biểu tượng **Play** ở trên cùng bảng điều khiển bên trái để vẽ lại đồ thị:

![ElasticSearch](/images/3/20.png?width=90pc)

Dữ liệu của bạn sẽ khác so, nhưng trong ví dụ này, chúng ta có thể thấy được sự phân bố số lượng giao dịch tại các kiểu kinh doanh bán lẻ, được xác định cho mỗi loại sản phẩm được bán nhiều nhất.

Bạn có thể thấy, Kibana là một công cụ rất mạnh cho phép bạn phân tích dữ liệu. Dành một chút thời gian để thử nghiệm với các loại đồ thị, trường và liên kết. Bạn có thể lưu lại các đồ thị mà bạn đã tạo và sau đó thêm chúng vào Dashboard sử dụng công vụ và biểu tượng trên trang Kibana.

Nếu bạn cần xóa các bản ghi, mapping và index từ miền ElasticSearch, bạn có thể chạy câu lệnh sau từ cửa sổ lệnh:

```bash
curl -XDELETE https://<ElasticsearchEndpoint>/transactions -k
```

Hãy thay thế giá trị ***ElasticsearchEndpoint*** từ tab **Cloudformation - Outputs**.

Bạn sẽ phải cần tạo lại các mapping, bằng cách chạy lại lệnh mapping mà bạn đã thực hiện ở bước trước. Bạn có thể thêm lại tập dữ liệu của bạn bằng cách chạy lại ứng dựng Consumer và Producer.

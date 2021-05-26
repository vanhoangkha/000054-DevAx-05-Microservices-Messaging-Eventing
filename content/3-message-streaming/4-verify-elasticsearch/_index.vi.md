+++
title = "Kiểm tra trong ElasticSearch"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4. </b>"
+++

**Kiểm tra Bản ghi trong ElasticSearch**

Trong bước này, chúng ta sẽ xác nhận rằng các bản ghi dữ liệu bán hàng đang được gửi đến ElasticSearch index một cách chính xác.

1. Mở **Amazon ElasticSearch Service console** trên trình duyệt web.
2. Trong **Amazon Elasticsearch Service dashboard**, tìm **idevelop domain**. Bên dưới **Searchable documents column**, bạn sẽ thấy **một số lớn hơn** không ở đây, nếu documents được khôi phục chính xác

![ElasticSearch](/images/3/12.png?width=90pc)

3. Nhấp vào liên kết **idevelop** để đi sâu vào cụm. 
4. Chọn tab **Indices** tab để hiển thị các chỉ số trong cluster. Bạn sẽ thấy chi tiết về số lượng tài liệu được lập chỉ mục, dung lượng lưu trữ cần thiết gần đúng và thông tin **Mappings** mà bạn đã đặt trước đó bằng cách sử dụng lệnh ```curl``` để thông báo cho ElasticSearch về các kiểu dữ liệu bạn đang sử dụng trong tài liệu của bạn cho chỉ mục này: 

![ElasticSearch](/images/3/13.png?width=90pc)

{{% notice note %}}
Nếu bạn không thấy bất kỳ tài liệu nào trong chỉ mục, hãy kiểm tra xem bạn đã làm theo tất cả các hướng dẫn ở trên và ứng dụng Kinesis Consumer trên thực tế đang hiển thị phản hồi như ***INFO: Processing xxxx records*** và không hiển thị bất kỳ lỗi nào.
{{% /notice %}}

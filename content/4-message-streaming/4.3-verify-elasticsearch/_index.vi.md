+++
title = "Kiểm tra trong ElasticSearch "
date = 2020
weight = 3
chapter = false
pre = "<b>4.3. </b>"
+++
#### Kiểm tra Bản ghi trong ElasticSearch 
Trong phần này, chúng ta sẽ xác nhận rằng các bản ghi dữ liệu bán hàng đang được gửi đến ElasticSearch index một cách chính xác.

1. Truy cập [**Amazon ElasticSearch Service console**](https://console.aws.amazon.com/esv3/home).
* Click **Dashboard**
* Click **Views all domains**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-001.png?featherlight=false&width=90pc)
2. Bên dưới cột **Searchable documents**, bạn sẽ thấy một số lớn hơn 0 nếu documents được lưu trữ đúng cách
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-002.png?featherlight=false&width=90pc)
3. Click **idevelop**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-003.png?featherlight=false&width=90pc)
4. Trong trang **idevelop**
* Click tab **Indices**
* Click **transactions**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-004.png?featherlight=false&width=90pc)
5. Bạn sẽ thấy chi tiết về số lượng tài liệu được lập chỉ mục, dung lượng lưu trữ cần thiết gần đúng và thông tin Mappings mà bạn đã đặt trước đó bằng cách sử dụng lệnh curl để thông báo cho ElasticSearch về các kiểu dữ liệu bạn đang sử dụng trong tài liệu của bạn cho chỉ mục này:
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-005.png?featherlight=false&width=90pc)



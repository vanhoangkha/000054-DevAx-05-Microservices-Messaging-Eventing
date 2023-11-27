+++
title = "Gửi các Bản ghi "
date = 2020
weight = 2
chapter = false
pre = "<b>4.2. </b>"
+++
#### Gửi các Bản ghi từ Producer đến Consumer
Trong phần này, chúng ta sẽ gửi hồ sơ bán hàng của mình qua hệ thống và xác nhận chúng đang được đẩy vào ElasticSearch.

1. Khởi động ứng dụng **Kinesis Producer** trên **Producer EC2 instance** bằng cách chạy lệnh dưới đây trong cửa số lệnh mà bạn đã kết nối SSH tới **Producer EC2 instance**
```
java -jar target/KinesisProducerApp-1.0.0.jar
```
Ứng dụng sẽ khởi động và sau đó bắt đầu gửi các sự kiện vào Kinesis stream. Bạn có thể xác nhận các bản ghi đang được ghi bằng tab giám sát trong Kinesis console và bằng đầu ra của ứng dụng trong cửa sổ lệnh:
![Send Records From The Producer To The Consumer](/images/4-message-streaming/4.2-send-records/send-records-001.png?featherlight=false&width=60pc)
2. Khởi động ứng dụng **Kinesis Consumer** trên **Consumer EC2 instance** bằng cách chạy lệnh dưới đây trong cửa số lệnh mà bạn đã kết nối qua SSH tới **Consumer EC2 instance**
```
java -jar target/KinesisConsumerApp-1.0.0.jar 
```
Nếu mọi thứ hoạt động, bạn sẽ nhận về phản hồi tương tự như bên dưới ở trong cửa số lệnh của Kinesis Consumer:
![Send Records From The Producer To The Consumer](/images/4-message-streaming/4.2-send-records/send-records-002.png?featherlight=false&width=60pc)
Hãy đế Producer và Consumer chạy một vài phút, sau đó hãy thoát chương trình với tổ hợp phím **CTRL + C**.


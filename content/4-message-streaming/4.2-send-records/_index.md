+++
title = "Sending Records"
date = 2020
weight = 2
chapter = false
pre = "<b>4.2. </b>"
+++
#### SEND RECORDS FROM THE PRODUCER TO THE CONSUMER
In this section, we will send our sales records through the system and confirm they are being pushed into **ElasticSearch**.

1. Start the **Kinesis Producer application** on the **Producer EC2 instance** issuing this command on the terminal that you have connected via SSH to the **Producer EC2 instance**
```
java -jar target/KinesisProducerApp-1.0.0.jar
```
Ứng dụng sẽ khởi động và sau đó bắt đầu gửi các sự kiện vào Kinesis stream. Bạn có thể xác nhận các bản ghi đang được ghi bằng tab giám sát trong Kinesis console và bằng đầu ra của ứng dụng trong cửa sổ lệnh:
![Send Records From The Producer To The Consumer](/images/4-message-streaming/4.2-send-records/send-records-001.png?featherlight=false&width=60pc)
2. Start the **Kinesis Consumer application** on the **Consumer EC2 instance** issuing this command on the terminal that you have connected via SSH to the **Consumer EC2 instance**
```
java -jar target/KinesisConsumerApp-1.0.0.jar 
```
If all is well, you should see output like this in the Kinesis Consumer terminal:
![Send Records From The Producer To The Consumer](/images/4-message-streaming/4.2-send-records/send-records-002.png?featherlight=false&width=60pc)
Let the Producer and Consumer run for a minute or so, then quit both applications using **CTRL + C**.

+++
title = "Gửi các Bản ghi"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++

**Gửi các Bản ghi từ Producer đến Consumer**

In this section, we will send our sales records through the system and confirm they are being pushed into **ElasticSearch**.

1. Khởi động ứng dụng **Kinesis Producer** trên **Producer EC2 instance** bằng cách chạy lệnh này cửa số lệnh mà bạn đã kết nối qua SSH với instance:

```bash
java -jar target/KinesisProducerApp-1.0.0.jar
```

{{% notice note %}}
Lưu ý rằng nếu bạn gặp lỗi ở đây, chỉ cần kiểm tra mã để đảm bảo bạn có thiết lập **region** chính xác vị trí bạn đang sử dụng.
Vui lòng giúp kiểm tra ```KinesisProducerApp.java``` và thay thế ```AP_SOUTHEAST_2``` bằng vùng bạn đang sử dụng.
{{% /notice %}}

Ứng dụng sẽ khởi động và sau đó bắt đầu gửi các sự kiện vào Kinesis stream. Bạn có thể xác nhận các bản ghi đang được ghi bằng tab giám sát trong Kinesis console và bằng đầu ra của ứng dụng trong cửa sổ lệnh:

```bash
===========================================================
           Starting from beginning of input file 
           at simulated date/time 23/12/2015 05:22:00
===========================================================
Published 190 records to stream source.
Published 456 records to stream source.
Published 454 records to stream source.
Published 345 records to stream source.
Published 305 records to stream source.
Published 490 records to stream source.
```

2. Khởi động ứng dụng **Kinesis Producer** trên **Consumer EC2 instance** bằng cách chạy lệnh này cửa số lệnh mà bạn đã kết nối qua SSH với instance:

```bash
java -jar target/KinesisConsumerApp-1.0.0.jar 
```

Nếu mọi thứ hoạt động, bạn sẽ nhận về phản hồi tương tự như bên dưới ở trong cửa số lệnh của Kinesis Consumer:

```bash
{"took":16362,"errors":false,"items":[{"create":{"_index":"transactions","_type":"transaction","_id":"AXhEyc5gmWx67_DERwVP","_version":1,"_shards":{"total":2,"successful":1,"failed":0},"status":201}},...
Mar 18, 2021 10:03:16 AM idevelop.demo.RecordProcessor processRecords
INFO: Processing 10000 records from shardId-000000000000
```

Hãy đế Producer và Consumer chạy một vài phút, sau đó hãy thoát chương trình với tổ hợp phím ```CTRL + C```.
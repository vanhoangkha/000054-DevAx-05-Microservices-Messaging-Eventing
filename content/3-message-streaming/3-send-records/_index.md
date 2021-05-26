+++
title = "Sending Records"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++

**SEND RECORDS FROM THE PRODUCER TO THE CONSUMER**

In this section, we will send our sales records through the system and confirm they are being pushed into **ElasticSearch**.

1. Start the **Kinesis Producer application** on the **Producer EC2 instance** by issuing this command on the terminal that you have connected via SSH to the instance:
```bash
java -jar target/KinesisProducerApp-1.0.0.jar
```

{{% notice note %}}
Note that if you get an error here, just check the code to make sure you have the correct **region** setup for the one you are using.
Please help to check the ```KinesisProducerApp.java``` and replace ```AP_SOUTHEAST_2``` with your Region.
{{% /notice %}}

The application will start up and then commence publishing events into the Kinesis stream. You can confirm the records are being written using the monitoring tab in the Kinesis console, and by the application’s output in the terminal:

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

2. Now the **Kinesis Consumer application** on the **Consumer EC2 instance** by issuing this command on the terminal that you have connected via SSH to the instance:
```bash
java -jar target/KinesisConsumerApp-1.0.0.jar 
```

If all is well, you should see output like this in the Kinesis Consumer terminal:

```bash
{"took":16362,"errors":false,"items":[{"create":{"_index":"transactions","_type":"transaction","_id":"AXhEyc5gmWx67_DERwVP","_version":1,"_shards":{"total":2,"successful":1,"failed":0},"status":201}},...
Mar 18, 2021 10:03:16 AM idevelop.demo.RecordProcessor processRecords
INFO: Processing 10000 records from shardId-000000000000
```

Let the Producer and Consumer run for a minute or so, then quit both applications using ```CTRL + C```.
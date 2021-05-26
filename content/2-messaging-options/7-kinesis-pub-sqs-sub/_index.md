+++
title = "Kinesis Publisher to SQS Subcriber"
date = 2021
weight = 7
chapter = false
pre = "<b>2.7. </b>"
+++

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. In this scenario, the browser will send draw points to a Kinesis stream. Normally, we would create a Kinesis consumer client and read the events from the stream directly and process them, and we will do this in our next scenario. But for this scenario, let’s assume we want the messages to be delivered to an SQS queue like we have done in our previous scenarios.

Depending on whether you chose to use the Eclipse envionment or AWS Cloud9 choose the appropriate lab instructions below:

**Contents**
1. [Option A: Eclipse Environment](a/)
2. [Option B: AWS Cloud9 Environment](b/)
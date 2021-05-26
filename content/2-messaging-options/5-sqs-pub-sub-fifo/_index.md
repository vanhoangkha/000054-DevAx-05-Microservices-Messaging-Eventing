+++
title = "SQS Publisher to SQS Subscriber with FIFO message ordering"
date = 2021
weight = 5
chapter = false
pre = "<b>2.5. </b>"
+++

In some applications, the indeterminate nature of the order of messages received in an SQS queue can be problemmatic. For example, if the queue contains CRUD operations, they need to be executed by subscribers in the order they were sent, because assumptions on the current state of the data may be made by subsequent operations on the data. In this scenario, we take a look at this requirement, using an SQS First-In-First-Out (FIFO) queue.

1. To start this demo, click the drop-down list on the website and choose **AWS SQS (FIFO) publisher to SQS subscriber**
2. When the page initialisation completes, draw a shape in the publisher canvas on the left, and observe the behaviour of the subscriber canvas on the right. As you draw, messages will be sent to an SQS FIFO queue containing information about the X & Y coordinates of the mouse. These messages will be received by the browser polling the same SQS queue - a single subscriber. The drawing points will be rendered in the **same order** that you drew them, and therefore, the **same order** they were published into the SQS FIFO queue. The FIFO behaviour has preserved the message ordering at the expense of throughput and latency. You may notice there is an additional delay observed before the messages start to be received. Again, this demo is not designed to highlight performance, and so this delay is caused by network and browser thread contention.

![SQS](/images/2/11.png?width=90pc)

For more information about SQS FIFO queues, see the AWS documentation at http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html

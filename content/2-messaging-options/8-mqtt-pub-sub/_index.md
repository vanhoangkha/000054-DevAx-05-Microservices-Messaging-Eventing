+++
title = "MQTT Publisher to MQTT Subcriber"
date = 2021
weight = 8
chapter = false
pre = "<b>2.8. </b>"
+++

The last messaging behaviour to explore is lightweight MQTT messaging, using AWS IoT as an ephemeral pub/sub bus. In this scenario, drawing events are published to a custom MQTT topic. This same custom topic is subscribed to by the receiving canvas, and points are rendered. The latency between the publish and subscribe is very low, but note that all messages sent on the bus are ephemeral - if the subscriber is not ‘listening’ when the publisher sends the message, the message is lost forever.

The architecture for this scenario looks like this:

![MQTT](/images/2/iotpubsubarchitecture.png?width=50pc)

To start this demo, click the drop-down list on the website and choose **AWS IoT MQTT publisher to MQTT subscriber**. When the page initialises, draw a shape on the MQTT Publisher canvas. The MQTT Subscriber canvas will update almost immediately in sync with the drawing on the publisher.

![SQS](/images/2/36.png?width=90pc)

To get an idea of the latency, click **Clear drawing** a few times and look at the Average Latency statistic.

Open another browser window, and browse to the demo website and arrange the windows so that you can see both the pages at the same time. Choose **AWS IoT MQTT publisher to MQTT subscriber slave** from the drop-down list on the second browser. Now, draw a shape on the MQTT Publisher. Notice that the shape appears on the MQTT Subscriber canvas on both pages at the same time. The MQTT messages automatically ‘fan-out’ to multiple subscribers. You do not need to pre-provision the subscriber pages ahead of time - they are dynamically subscribed to the same bus when they connect. Unlike SQS for example, where you need to provision the SQS queue ahead of time.

To demonstrate this, ask one of your peers to also browse to your website and choose the slave page. Your drawings on the MQTT Publisher canvas will appear on all MQTT Subscriber canvases as you draw.s
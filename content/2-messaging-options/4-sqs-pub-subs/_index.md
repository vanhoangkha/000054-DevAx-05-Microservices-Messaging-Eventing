+++
title = "SQS Publisher to Multiple SQS Subscribers"
date = 2021
weight = 4
chapter = false
pre = "<b>2.4. </b>"
+++

In this scenario, we take a look at the behaviour when multiple subscribers are polling the same SQS queue for messages. In this case, the subscribers will compete for messages, and some messages will be claimed by one subscriber, some by the others. If the data that is being sent is considered a continuous stream of related data, the different subscribers would need to coordinate in order to make sense of the messages if they are part of a continuum. If there is no coordination, then the order of reception of the messages, and the subscriber that receives each one, is indeterminate. This pattern is appropriate when there is no coordination needed between the subscribers in order to successfully process the messages in the queue, and when the order of messages consumed by each individual subscriber and the fact that any particular subscriber may miss messages, is not important to the design of the application. The architecture looks like this:

![SQS](/images/2/sqspubmultisub-architecture.png?width=50pc)

1. To start the demo, click the drop-down list on the website and choose **AWS SQS (standard) publisher to multiple SQS subscribers**.
2. When the page initialisation completes, draw a shape in the publisher canvas on the left, and observe the behaviour of the three subscriber canvases on the right. As you draw, messages will be sent to an SQS queue containing information about the X & Y coordinates of the mouse. These messages will be received by the browser polling the same SQS queue with 3 different subscribers, each rendering the events they receive on their own canvas.

![SQS](/images/2/10.png?width=90pc)

*Notice how the points are drawn out-of-order on the right-hand side and no single canvas has the complete drawing?*

Points are drawn out-of-order because messages received via an SQS queue are received in an indeterminate order. This is a characteristic of the standard SQS queue. And since each canvas is polling the same SQS queue, they are competing for the messages in the queue, and will arbitrarily receive the points events and render them, stealing them from the other canvases.

Regardless of which canvas subscriber receives the drawing point message, all of the sent messages will be received - you can confirm this by comparing the **Messages Sent** counter against the **Messages Received** counter in the publisher statistics panel - they will eventually be identical.

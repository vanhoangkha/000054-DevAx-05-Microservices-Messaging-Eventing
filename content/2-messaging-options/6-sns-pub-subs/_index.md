+++
title = "SNS Publisher to Multiple SQS Subcribers"
date = 2021
weight = 6
chapter = false
pre = "<b>2.6. </b>"
+++

In [Scenario 2.4](../4-sqs-pub-subs/), we saw how multiple subscribers reading from the same SQS queue could ‘steal’ messages from each other, so that no single subscriber saw all the messages in the queue. Sometimes it is a requirement that multiple subscribers to a queue receive all messages, so some mechanism for publishing a single message to multiple queues is required. The solution is to use an intermediatry to ‘fan out’ the message to multiple subscribers. The intermediary in this case, is Amazon SNS, the Simple Notification Service. Amazon SNS allows an application to send a single message intent, and have that message delivered to one or more targets. The target(s) of the message can be an HTTP endpoint, a mobile device, or an SQS queue - and in fact, multiples and any combination of these. The architecture for this scenario looks like this:

![SNS](/images/2/snssqsarchitecture.png?width=50pc)

**Contents**
- [How is the SNS fan-out to multiple SQS queues configured?](#how-is-the-sns-fan-out-to-multiple-sqs-queues-configured)
- [How are the messages sent to an SNS topic from Javascript?](#how-are-the-messages-sent-to-an-sns-topic-from-javascript)

1. To start this demo, click the drop-down list on the website and choose **AWS SNS publisher to multiple SQS subscribers**
2. When the page initialisation completes, draw a shape on the publisher canvas. Notice how the shape is rendered on the right-hand subscriber canvases, with each canvas showing an identical and full representation of the publisher canvas? This is because the draw points are sent to an Amazon SNS topic, which ‘fans out’ to three Amazon SQS queues, so that each queue receives every message. Eventually, all three subscribers will show the full shape, although they will have received the draw point messages in different orders to each other, and to the order in which they were drawn.

![SNS](/images/2/12.png?width=90pc)

Also note that the Messages Received statistic is 3 times the Messages Sent statistic, which is to be expected if the messages sent are ‘fanned out’ to three queues.

#### How is the SNS fan-out to multiple SQS queues configured?
Adding one or more SQS queues as targets for SNS notifications is simple. Lets take a look at this now.

3. To review the configuration used in this Lab, open the **SNS console** in the browser.
4. Click the Topics link on the navigation panel to reveal the **iDevelopMessageDuplicator** SNS topic:

![SNS](/images/2/13.png?width=90pc)

5. Click on the arn to drill down into the subscriptions for this SNS topic:

![SNS](/images/2/14.png?width=90pc)

You will see three subscribers - one for each of the three SQS queues **arn:aws:sqs:us-west-2:277844687642:idevelop_drawqueue_XX** where **XX** is from ```1``` to ```3```.

When a notification is sent to the SNS topic, it is automatically routed through to each of the subscribers, in this case, to three SQS queues. When the browser implementation sends draw points to the SNS topic, they too are routed through to three SQS queues, which are each subscribed to by the web page implementation, and the points rendered on the three subscriber canvases.

#### How are the messages sent to an SNS topic from Javascript?
This snippet of Javascript shows how messages are sent to the SNS topic:

```js
var params = {
	Message: JSON.stringify(point),
	TopicArn: "<SNS Topic ARN>"
};

snsInstance.publish(params, function(err, data){});
```

Where can I find more information about the AWS SDK for Javascript and SNS?

http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html

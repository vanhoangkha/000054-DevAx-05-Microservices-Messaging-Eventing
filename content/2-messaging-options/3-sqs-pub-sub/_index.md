+++
title = "SQS Publisher to SQS Subcriber"
date = 2021
weight = 3
chapter = false
pre = "<b>2.3. </b>"
+++

In this scenario, we will make use of these queues and take a look at a simple use case where we have a single publisher pushing messages into an SQS queue, and a single subscriber reading from the same queue. The architecture looks like this:

![SQS](/images/2/sqspubsub-architecture.png?width=50pc)

The web page uses the AWS SDK for Javascript in the browser, and authenticates anonymously with Amazon Cognito to obtain scoped, temporary credentials with which it can sign requests to connect into the Amazon SQS service, to publish and subscribe to the same SQS queue.

In this demo, the publisher is represented on the left grey canvas, the subscriber on the right. Using your mouse, draw a shape by clicking down with the left mouse button while you move the mouse to leave a trail of pixels. As you draw, messages will be sent to an SQS queue containing information about the X & Y coordinates of the mouse. These messages will be received by the browser polling the **same** SQS queue, and rendered on the right-hand canvas.

**Contents**
- [Notice how the points are drawn out-of-order on the right-hand side?](#notice-how-the-points-are-drawn-out-of-order-on-the-right-hand-side)
- [What is causing the delay between send and receive?](#what-is-causing-the-delay-between-send-and-receive)
- [How can I get an idea of the latency between send and receive?](#how-can-i-get-an-idea-of-the-latency-between-send-and-receive)
- [Javascript code to obtain credentials from Amazon Cognito](#javascript-code-to-obtain-credentials-from-amazon-cognito)
- [How are the messages sent into the queue from javascript?](#how-are-the-messages-sent-into-the-queue-from-javascript)
- [Where can I find more information about the AWS SDK for Javascript and SQS?](#where-can-i-find-more-information-about-the-aws-sdk-for-javascript-and-sqs)
- [Inspect the messages in the SQS queue](#inspect-the-messages-in-the-sqs-queue)

#### Notice how the points are drawn out-of-order on the right-hand side?
This is because messages received via an SQS are received in an indeterminate order. This is a characteristic of the **standard SQS queue**.

For example, draw a triangle:

![SQS](/images/2/7.png?width=90pc)

Notice the statistics panel on the publisher side - it shows the SQS messages it has sent, and those where the HTTP call has successfully completed. When you stop drawing, these two numbers will be the same after a moment or two.

On the right you will see the subscriber representation. The grey canvas will eventually mirror the drawing in the publisher canvas. It will take a few moments for all the messages to be received and rendered on the subscriber canvas. Eventually, the statistics in the subscriber panel will show the same number of messages received as the messages sent statistics in the publisher panel.

What you will notice in this demo is that the shape will render the drawing points on the subscriber panel in a different order than the order they were drawn. Regardless, eventually, every drawing point will be rendered.

#### What is causing the delay between send and receive?
Because of the nature of the threading model in the browser, and contention on the network stack through the browser, there will be some delay and competition between the send and receive sides of the demonstration. Amazon SQS is highly scalable and performant - but this demo is not designed to illustrate performance. It is designed to highlight the out-of-order message reception. There is no coordination between publishing and subscribing in this demo and the web browser doesn’t implement an ability to ‘spawn threads’ to run the two tasks simulataneously, so they compete.

#### How can I get an idea of the latency between send and receive?
If you click the **Clear drawing** button a few times, and look at the **Average Latency** statistic on the subscriber panel, you will get an idea of the end-to-end latency of the demo. When you click the **Clear drawing** button, a single message is sent to the SQS Queue. This will reduce the contention and the impact of threading in the browser from the timing of the message pub/sub.

#### Javascript code to obtain credentials from Amazon Cognito
In order for the demonstrations in this lab to be able to connect to and query your AWS environment (for example, to send messages to queues) the Javascript code running in the browser needs to obtain credentials. You should never embed Access Keys/Secret Access Keys into your applications, so another mechanism for the web page to dynamically obtain credentials is required. Amazon Cognito provides a mechanism to vend temporary credentials to untrusted environments, such as web pages. In these demonstrations, we will use the ‘unauthenticated’ or ‘anonymous’ access mode with Amazon Cognito. In this mode, the web page can obtain the temporary credentials without the user having to provide authentication credentials.

Obtaining the credentials only takes a few lines of code. The lab setup has already configured a Cognito Identity Pool for you, and you updated the *aws-config.js* file previously, to add in the Cognito IdentityPoolId value to make it available to the Javascript code.

Here’s a snippet of the code that obtains the credentials, taken from the **Messaging/scripts/cognito-bootstrap.js** file in the zip bundle:

```js
var params = {
	IdentityPoolId : config.Cognito.IdentityPoolId
};

AWS.config.region       = config.Region;
AWS.config.credentials  = new AWS.CognitoIdentityCredentials(params);

AWS.config.credentials.get(function(err)
{
	if (!err)
	{
		console.log("Cognito Identity Id: " + AWS.config.credentials.identityId);
		connectedCallback(configuration);
	}
	else
	{
		console.log("Cognito Error: " + err );
	}
});
```

You simply need to provide the Cognito Identity Pool Id to the call to **AWS.config.credentials.get** from the Javascript SDK, and when the call succeeds, the SDK will automatically consume the temporary Access Key/Secret Access Key returned from the call. From this point on, every subsequent call through the Javascript SDK to AWS endpoints is signed using those credentials.

#### How are the messages sent into the queue from javascript?
This snippet of Javascript shows how messages are published into an SQS queue:

```js
var params = {
	MessageBody: JSON.stringify(point),
	QueueUrl: "<The Queue Url>"
}

sqsInstance.sendMessage(params, function(err, data){});
```

***How are the messages received from the queue from Javascript?***

This snippet of Javascript shows how messages are received from an SQS queue:

```js
var params = {
	MaxNumberOfMessages: 10,
	QueueUrl: "<The Queue Url>",
	VisibilityTimeout: 20,
	WaitTimeSeconds: 20
};

sqsInstance.receiveMessage(params, function(err, data){});
```

#### Where can I find more information about the AWS SDK for Javascript and SQS?

http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html

#### Inspect the messages in the SQS queue
Using the AWS console, it is possible to view messages that have been published into an SQS queue but not yet received by a subscriber. This is a handy feature during design and debugging of an application.

1. Open the **SQS console** in the browser, in a different browser tab so both are active at the same time.

You will see the SQS Queues that have been provisioned for you by the EE setup. Select **idevelop_drawqueue_1** and from the **Queue Actions** menu, **View/Delete messages**:

![SQS](/images/2/8.png?width=90pc)

2. Click **Start Polling for Messages**. In another browser tab, draw more points on the publisher canvas, then switch back to the AWS console tab:

![SQS](/images/2/9a.png?width=90pc)

![SQS](/images/2/9b.png?width=90pc)

Viewing the messages this way does not consume them, they are still available to subscribers to process. But it is a helpful tool, to check that messages are flowing through the queue(s) as expected during the development phase.

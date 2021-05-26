+++
title = "Eclipse Environment"
date = 2021
weight = 1
chapter = false
pre = "<b>Option A. </b>"
+++

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. In this scenario, the browser will send draw points to a Kinesis stream. Normally, we would create a Kinesis consumer client and read the events from the stream directly and process them, and we will do this in our next scenario. But for this scenario, let’s assume we want the messages to be delivered to an SQS queue like we have done in our previous scenarios.

Since there is no in-built way to route messages that have been published to a Kinesis stream into an SQS queue, we need an intermediary to do that processing/routing for us. In this scenario, we will implement the intermediary as an AWS Lambda function that will create a bridge between the Kinesis stream, and the SQS queue we want to target.

The architecture for this scenario looks like this:

![SQS](/images/2/kinesislambdasqsarchitecture.png?width=50pc)

Similar to a previous scenario, messages are rendered by a single subscriber to an Amazon SQS queue. But in this case, the messages are published to a Kinesis stream, and then transferred to the SQS queue by the Lambda function.

**Contents**
- [Create an AWS Lambda function that implements an Amazon Kinesis event consumer](#create-an-aws-lambda-function-that-implements-an-amazon-kinesis-event-consumer)
- [Update the Lambda function implementation to publish received events to an SQS queue](#update-the-lambda-function-implementation-to-publish-received-events-to-an-sqs-queue)


1. To start this demo, click the drop-down list on the website and choose **AWS Kinesis publisher to SQS subscriber**. When the page initialises, draw a shape on the Kinesis Publisher canvas. Notice that the **Messages Sent** counter increments, but no shape is rendered on the SQS Subscriber canvas? This is because the messages are being published into the Kinesis stream, but no Kinesis consumers are processing the message yet:

![SQS](/images/2/15.png?width=90pc)

Follow the steps below to implement the Lambda function.

#### Create an AWS Lambda function that implements an Amazon Kinesis event consumer

2. In the **Eclipse IDE**, click the **AWS** icon to reveal the AWS menu, and select the option **New AWS Lambda java Project...**

![SQS](/images/2/16.png?width=90pc)

3. In the dialog that appears, for **Project name** type ```kinesissam-202103-LambdaFunction-XXXXXXXXX```
4. For **Input Type** select `Kinesis Event` from the drop-down list.

![SQS](/images/2/17.png?width=90pc)

5. Click **Finish**

We will use the default implementation to test that the configuration is correct before updating the implementation to peform a specific task.

6. Right-click on the **LambdaFunctionHandler.java** file and select the **Amazon Web Services** menu item. From the subitem that appears, choose **Upload function to AWS Lambda...**

![SQS](/images/2/18.png?width=90pc)

7. Make sure the **Asia Pacific(Tokyo)** region is selected, and for **Create a new Lambda function**: enter ```kinesissam-202103-LambdaFunction-XXXXXXXXX```.
8. Click **Next**.

![SQS](/images/2/19.png?width=90pc)

9. Enter a **Description** such as ```Lab 5 Kinesis to SQS```.
10. For **IAM Role** select ```LambdaKinesisToSQSRole``` which has been provisioned for you by the lab setup.
11. Click **Finish**. The Lambda function will be uploaded to your AWS account, ready to test. This may take a moment or two.

While you are waiting for the upload to complete, take a quick look at the simple implementation the IDE has created for you.

12. Open the **LambdaFunctionHandler** class. It looks like this:

```java
public class LambdaFunctionHandler implements RequestHandler<KinesisEvent, Integer> {

    @Override
    public Integer handleRequest(KinesisEvent event, Context context) {
        context.getLogger().log("Input: " + event);

        for (KinesisEventRecord record : event.getRecords()) {
            String payload = new String(record.getKinesis().getData().array());
            context.getLogger().log("Payload: " + payload);
        }

        return event.getRecords().size();
    }
}
```

This implementation will simply log the input event received by the Lambda function, and then for each record in the batch, it will log the contents. This is a good starting point to allow us to confirm our configuration is correctly reading data from the Kinesis stream.

13. Open the **Kinesis console** in the browser and select **Go to the Streams console**

![SQS](/images/2/20.png?width=90pc)

14. You will see two streams - both have been provisioned by the lab template during setup. Click on **iDevelopDrawingData**

![SQS](/images/2/21.png?width=90pc)

15. Select the **Monitoring** tab and scroll down to reveal the **Put Records (Bytes) - Sum** graph.

![SQS](/images/2/22.png?width=90pc)

Around this location will be other statistics about the stream. Notice that the **Incoming data - sum (Count)** matches the same number you see in our demo website, under **Messages Sent**? You may need to move your mouse over the graph to see the actual data volume. In the example shown here, you would point your mouse at the end of the line to reveal the count (showing 118). The count of the two points on the graph will equal the **Messages Sent** metric on our website (last image, showing the Messages Sent from our website. 118)

![SQS](/images/2/23.png?width=90pc)

![SQS](/images/2/25.png?width=90pc)

16. When the Lambda function has finished uploading, open the **AWS Lambda console** in the browser. You will see the new Lambda function you just uploaded, ```KinesisToSQS```. Click on it to manage its configuration.
17. Under the Designer dashboard, you will find the **Add triggers** section.

![SQS](/images/2/26.png?width=90pc)

18. Click **Kinesis** from the list of triggers. It opens up Trigger configuration.
19. For **Kinesis stream** select ```iDevelopDrawingData``` from the list
20. For **Starting position** select ```Trim horizon```. Leave the **Batch size** at ```100```.

![SQS](/images/2/27.png?width=90pc)

21. Click **Add** and then click **Save**. The Kinesis trigger will be enabled for this Lambda function.

![SQS](/images/2/28.png?width=90pc)

22. It will take a moment or two for the Lambda function to be attached to the Kinesis stream. Wait a minute, and then click the **Refresh** icon on the **Kinesis** pannel. When the Lambda function is wired-up, you will see a message saying *Last processing result: OK*.

![SQS](/images/2/29.png?width=90pc)

23. Click the **Monitoring** tab. The draw points that have been sent into the Kinesis stream will have been consumed by the Lambda function, and you will see CloudWatch metrics indicating this activity. You will see activity in the *Invocation count* and *Invocation duration graphs*. Check that you don’t see any *Invocation error*s.

{{% notice note %}}
Depending on how many draw points you emited into the Kinesis stream, you will see a different number for *Invocation count*. When we enabled the trigger, we set the Batch size to 100 (which was the default). So the Lambda function will be invoked with up to 100 records at a time. If you sent 225 messages into the stream, this will be consumed by 3 invocations of the Lambda function - Math.ceil(225/100) == 3.
{{% /notice %}}

![SQS](/images/2/30.png?width=90pc)

24. Click the **View logs in CloudWatch** button to open the CloudWatch Log Stream for this Lambda function. Click on the first log stream in the list and review the logs listed:

![SQS](/images/2/31.png?width=90pc)

25. Scroll through the events in the CloudWatch Log output. These events have been written to the log by our Lambda function code:

![SQS](/images/2/32.png?width=90pc)

When writing your Lambda function code in Java, you can emit log strings into CloudWatch logs by simply calling the Logger’s .log() method:

```bash
context.getLogger().log("Log message to emit: " + someMessage);
```

#### Update the Lambda function implementation to publish received events to an SQS queue
Getting back to our scenario, we want to read the messages out of the Kinesis stream, and publish them into an SQS queue, where our browser implementation can read them and draw them on the subscriber’s canvas.

26. In the **Eclipse IDE**, replace the **entire contents** of the ```/KinesisToSQS/src/main/java/com/amazonaws/lambda/demo/LambdaFunctionHandler.java``` file with the following code:

```java
package com.amazonaws.lambda.demo;

import java.awt.geom.AffineTransform;
import java.awt.geom.Point2D;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import com.amazonaws.services.lambda.runtime.events.KinesisEvent;
import com.amazonaws.services.lambda.runtime.events.KinesisEvent.KinesisEventRecord;
import com.amazonaws.services.sqs.AmazonSQS;
import com.amazonaws.services.sqs.AmazonSQSClientBuilder;
import com.amazonaws.services.sqs.model.SendMessageBatchRequest;
import com.amazonaws.services.sqs.model.SendMessageBatchRequestEntry;
import com.fasterxml.jackson.databind.ObjectMapper;

public class LambdaFunctionHandler implements RequestHandler<KinesisEvent, Integer> {

	final int MAX_SQS_BATCH_SIZE = 10;

	private AmazonSQS sqs = AmazonSQSClientBuilder.defaultClient();

    @Override
    public Integer handleRequest(KinesisEvent event, Context context) {

    		Iterator<KinesisEventRecord> iterator = event.getRecords().iterator();    		
    		AffineTransform transformer = new AffineTransform(1, 0, 0, -1, 0, 400);
    		ObjectMapper mapper = new ObjectMapper();
    		Point2D dstPoint = new Point2D.Double();
    		boolean sendMessagesToSQS = true;
    		
    		try
    		{
    			//
    			// Read the SQS Queue Target from the Lambda Environment Variables
    			//
    			String sqsUrl = System.getenv("TargetSQSUrl");
    			if ( sqsUrl == null || sqsUrl.isEmpty() )
    			{
    				context.getLogger().log("WARNING:: Environment Variable [TargetSQSUrl] is not set. No messages will be sent via SQS");
    				sendMessagesToSQS = false;
    			}
    				
	    		while ( iterator.hasNext() )
	    		{
	    			int messageCounter = 0;
	
	        		// Prepare a batch request to write all the messages we have received in this invocation
	        		Collection<SendMessageBatchRequestEntry> entries = new ArrayList<SendMessageBatchRequestEntry>();
	
	        		while ( iterator.hasNext() && messageCounter++ < MAX_SQS_BATCH_SIZE )
	        		{
		            String payload = new String(iterator.next().getKinesis().getData().array());
		            context.getLogger().log("Payload: " + payload);
		            DrawPoint transformedPoint = new DrawPoint();
	
		            try {
	
						transformedPoint = mapper.readValue(payload, DrawPoint.class);
			            // Transform the point
			            Point2D srcPoint = new Point2D.Double(transformedPoint.getX(), transformedPoint.getY());	            
			            transformer.transform(srcPoint, dstPoint);
	
			            // Update the payload
			            transformedPoint.setX((int)dstPoint.getX());
			            transformedPoint.setY((int)dstPoint.getY());
	
		                 // Add this payload into our batch
		        		    	SendMessageBatchRequestEntry entry = new SendMessageBatchRequestEntry(
		        		    			"msg_" + messageCounter,
		        		    			mapper.writeValueAsString(transformedPoint)
		        		    			);
	
		        		    	entries.add(entry);
					}
		            catch (Exception e)
		            {
		            		context.getLogger().log("Unable to deserialise " + payload + " as a DrawPoint! " + e.getMessage());					
					}
	        		}
	
	        		if ( sendMessagesToSQS && entries.size() > 0 )
	        		{
	        			// We have reached the end of the records or we have reached the maximum	        		
		        		// batch size allowed for SQS, so we need to send our entries
		        		context.getLogger().log("Sending batch of " + (messageCounter - 1) + " events to SQS...");
		
		        		SendMessageBatchRequest batch = new SendMessageBatchRequest()
		        				.withQueueUrl(sqsUrl);
		
		            batch.setEntries(entries);
		
		            // Perform the message sending
		            sqs.sendMessageBatch(batch); 
	        		}
	    		}
    		}
		catch (Exception e)
        {
        		context.getLogger().log("EXCEPTION::Aborting Lambda processing");
        		context.getLogger().log(e.getStackTrace().toString());
		}
    		
        return event.getRecords().size();
    }

    // Inner class must be marked as static in order for the JSON mapper to deserialise
	private static class DrawPoint {

		private int x;
		private int y;
		private long timestamp;
		private boolean clear;
		
		public int getX() {
			return x;
		}
		public void setX(int x) {
			this.x = x;
		}
		public int getY() {
			return y;
		}
		public void setY(int y) {
			this.y = y;
		}
		public long getTimestamp() {
			return timestamp;
		}
		public void setTimestamp(long timestamp) {
			this.timestamp = timestamp;
		}
		public boolean isClear() {
			return clear;
		}
		public void setClear(boolean clear) {
			this.clear = clear;
		}	
	}
}
```

You can also download this file from [**here**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/LambdaFunctionHandler.java).

27. Upload the Lambda function again using the Eclipse IDE.
28. The Lambda function implementation expects the SQS Queue Url to be passed in via an Environment Variable. Before the function will be able to send messages to the SQS queue, you need to configure this variable.

Once the upload is complete, on the AWS Lambda console, click the **Configuration** tab for the kinesissam-202103-LambdaFunction-PXLJKP8VVOJB Lambda function. Scroll down to the **Environment variables** section (if you don’t see it, click the name of your Lambda function in the Designer dashboard) and add a new variable with the key ```TargetSQSUrl``` and set the value to the ```QueueUrl1``` from the lab **Additional Info panel**. Click **Save**.

![SQS](/images/2/33.png?width=90pc)

29. Now go back to our demo website and draw a new shape to emit events into the Kinesis stream.

![SQS](/images/2/34.png?width=90pc)

The drawing points should now be received via the SQS queue, and rendered on the canvas. Notice that the shape you draw is rendered inversed in the Y-dimension? This is because the implementation we used for the Lambda function executes a transform on the drawing points, just to demonstrate how we can manipulate the data in the Kinesis stream before passing it to the SQS queue.

Take a few moments to review the handler implementation code to ensure you understand how it works.
+++
title = "Kinesis Publisher to SQS Subcriber"
date = 2020
weight = 6
chapter = false
pre = "<b>3.6. </b>"
+++
#### Kinesis Publisher to SQS Subcriber

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. In this scenario, the browser will send draw points to a Kinesis stream. Normally, we would create a Kinesis consumer client and read the events from the stream directly and process them, and we will do this in our next scenario. But for this scenario, let’s assume we want the messages to be delivered to an SQS queue like we have done in our previous scenarios.

Since there is no in-built way to route messages that have been published to a Kinesis stream into an SQS queue, we need an intermediary to do that processing/routing for us. In this scenario, we will implement the intermediary as an AWS Lambda function that will create a bridge between the Kinesis stream, and the SQS queue we want to target.

The architecture for this scenario looks like this:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesislambdasqsarchitecture.png?featherlight=false&width=70pc)
Similar to a previous scenario, messages are rendered by a single subscriber to an Amazon SQS queue. But in this case, the messages are published to a Kinesis stream, and then transferred to the SQS queue by the Lambda function.

o start this demo, click the drop-down list on the website and choose AWS Kinesis publisher to SQS subscriber. When the page initialises, draw a shape on the Kinesis Publisher canvas. Notice that the Messages Sent counter increments, but no shape is rendered on the SQS Subscriber canvas? This is because the messages are being published into the Kinesis stream, but no Kinesis consumers are processing the message yet:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-001.png?featherlight=false&width=90pc)

#### Create an AWS Lambda function that implements an Amazon Kinesis event consumer

1. Open **Eclipse IDE**.
* Find the AWS Icon and click it to reveal the menu
* Click **New AWS Lambda java Project…**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-002.png?featherlight=false&width=90pc)
2. In the **Create a new AWS Lambda Java project** section
* In the **Project name** section, type ```KinesisToSQS```
* In the **Input Type** section, select **Kinesis Event**
* Click **Finish**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-003.png?featherlight=false&width=90pc)
3. Right-click on the file whose the path is **KinesisToSQS\src\main\java\com\amazonaws\lambda\demo\LambdaFunctionHandler.java**
* Click **Amazon Web Services**
* Click **Upload function to AWS Lambda…**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-004.png?featherlight=false&width=90pc)
4. In the **Select Target AWS Lambda Function** section
* In the **Select the AWS Region** section, select your **Region**
* In the **Create a new Lambda function** section, type ```Kinesissam-LambdaFunction```
* Click **Next**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-005.png?featherlight=false&width=90pc)
5. In the **Function Configuration**section
* In the **Description** section, type ```Lab 5 Kinesis to SQS```
* In the **IAM Role** section, select **LambdaKinesisToSQSRole**
* Click **Finish**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-006.png?featherlight=false&width=90pc)
6. While you are waiting for the upload to complete, take a quick look at the simple implementation the IDE has created for you. Open class **LambdaFunctionHandler**. Chúng sẽ tương tự như sau:
```
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
7. Go to [**AWS Kinesis console**](https://console.aws.amazon.com/kinesis/home)
* Click **Data streams**
* You will see two streams - both have been provisioned by the lab template during setup. Click **iDevelopDrawingData**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-007.png?featherlight=false&width=90pc)
8. Select **Monitoring** tab and scroll down to reveal the **Incoming data - sum (Count)** graph
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-008.png?featherlight=false&width=90pc)
Around this location will be other statistics about the stream. Notice that the **Incoming data - sum (Count)** matches the same number you see in our demo website, under **Messages Sent**? You may need to move your mouse over the graph to see the actual data volume. In the example shown here, you would point your mouse at the end of the line to reveal the count (showing **206**). The count of the two points on the graph will equal the **Messages Sent** metric on our website (last image, showing the Messages Sent from our website. **206**)
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-009.png?featherlight=false&width=90pc)
9. Go to [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home)
* Click **Functions**
* Type ```Kinesissam-LambdaFunction``` to the search bar, type **Enter**
* Click **Kinesissam-LambdaFunction**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-010.png?featherlight=false&width=90pc)
10. Click **Add trigger**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-011.png?featherlight=false&width=90pc)
11. In the **Add trigger** page
* Select **Kinesis**
* In the **Kinesis stream** section, select **iDevelopDrawingData**
* In the **Starting position** section, select **Trim horizon**
* Click **Add**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-012.png?featherlight=false&width=90pc)
12. It will take a moment or two for the Lambda function to be attached to the Kinesis stream. Wait a minute, and then click **Refresh Icon**
* Click **Details**. When the Lambda function is wired-up, you will see a message saying **Last processing result: OK**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-013.png?featherlight=false&width=90pc)
13. Select tab **monitor**. The draw points that have been sent into the Kinesis stream will have been consumed by the Lambda function, and you will see CloudWatch metrics indicating this activity. You will see activity in the Invocation count and Invocation duration graphs. Check that you don’t see any Invocation errors.
{{% notice note %}} 
Depending on how many draw points you emited into the Kinesis stream, you will see a different number for Invocation count. When we enabled the trigger, we set the Batch size to 100 (which was the default). So the Lambda function will be invoked with up to 100 records at a time. If you sent 225 messages into the stream, this will be consumed by 3 invocations of the Lambda function - Math.ceil(225/100) == 3.
{{% /notice %}}
* Click **View logs in CloudWatch** to open the CloudWatch Log Stream for this Lambda function.
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-014.png?featherlight=false&width=90pc)
14. Click first **Log stream**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-015.png?featherlight=false&width=90pc)
15. Scroll through the events in the CloudWatch Log output. These events have been written to the log by our Lambda function code:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-016.png?featherlight=false&width=90pc)

When writing your Lambda function code in Java, you can emit log strings into CloudWatch logs by simply calling the Logger’s .log() method:
```
context.getLogger().log("Log message to emit: " + someMessage);
```
#### Update the Lambda function implementation to publish received events to an SQS queue

Getting back to our scenario, we want to read the messages out of the Kinesis stream, and publish them into an SQS queue, where our browser implementation can read them and draw them on the subscriber’s canvas.

1. In the Eclipse IDE, replace the entire contents of the **KinesisToSQS/src/main/java/com/amazonaws/lambda/demo/LambdaFunctionHandler.java** file with the following code:
```
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
* Save
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-017.png?featherlight=false&width=90pc)
2. Open **Command Prompt** and navigate to the directory of **KinesisToSQS** project
* Execute the command ```mvn package``` to build
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-018.png?featherlight=false&width=60pc)
3. Go to [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home)
* Click **Functions**
* Type ```Kinesissam-LambdaFunction``` to the search bar, type **Enter**
* Click **Kinesissam-LambdaFunction**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-019.png?featherlight=false&width=90pc)
4. In the **Code soure** section
* Click Upload from
* Click **.zip or .jar file**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-020.png?featherlight=false&width=90pc)
5. Click **Upload**, Select file **demo-1.0.0.jar** in **target** folder of the **KinesisToSQS** project we built in step 2
* Click **Save**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-021.png?featherlight=false&width=90pc)
6. Click tab **Configuration**
* Click **Environment variables**
* Click **Edit**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-022.png?featherlight=false&width=90pc)
7. In the **Edit Environment variables** page
* Click **Add environment variable**
* In the **Key** section, type **TargetSQSUrl**
* In the **Value** section, type the value of **QueueUrl1** we saved in the step 3 of section 3.1
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-023.png?featherlight=false&width=90pc)
8. Go back to our demo website and draw a new shape to emit events into the Kinesis stream.
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-024.png?featherlight=false&width=90pc)

The drawing points should now be received via the SQS queue, and rendered on the canvas. Notice that the shape you draw is rendered inversed in the Y-dimension? This is because the implementation we used for the Lambda function executes a transform on the drawing points, just to demonstrate how we can manipulate the data in the Kinesis stream before passing it to the SQS queue.

Take a few moments to review the handler implementation code to ensure you understand how it works.
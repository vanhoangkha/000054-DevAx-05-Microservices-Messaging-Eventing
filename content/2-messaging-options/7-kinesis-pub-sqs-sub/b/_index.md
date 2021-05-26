+++
title = "AWS Cloud9 Environment"
date = 2021
weight = 2
chapter = false
pre = "<b>Option B. </b>"
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

2. In the **Cloud9 IDE**, we will need to setup SAM to deploy the lambda function for a kinesis stream into your AWS account.

Make sure your environment has SAM installed - ```sam --version``` if not, action the below. This should be version 0.53.0 or higher. To do this open a new terminal

```bash
sudo pip install --upgrade aws-sam-cli
```

Now we need to setup the SAM requirements to build your lambda stack. This is the same as you creating the Lamdba project in eclipse. To do this through cloud9 we will be using SAM to deploy it. Copy the following to your server: [**kinesis_setup.zip**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/kinesis_setup.zip)

{{%attachments title="Kinesis setup file" pattern=".*(zip)"/%}}

Lets curl this over to cloud9:

```bash
wget  http://workshops.devax.academy/monoliths-to-microservices/module5/files/kinesis_setup.zip
```

```bash
mkdir kinesis_setup && cd kinesis_setup
sam init --location ~/environment/kinesis_setup.zip
```

This provides a full stack for a kinesis stream trigger, but in our example we already have a stream created for you in the original lab setup. Lets update the template.yml file to only deploy the lambda function :

```yml
# edit template.yml so it looks like this:
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: A kinesis example deployed into lambda using SAM

Resources:
  LambdaFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: ./
      MemorySize: 1028
      Handler: com.amazonaws.lambda.demo.LambdaFunctionHandler
      Runtime: java8
      Timeout: 60

      Tracing: Active
      Role: !Sub "arn:aws:iam::${AWS::AccountId}:role/LambdaKinesisToSQSRole"
Outputs:
  FunctionName:
    Description: "Function name"
    Value: !Ref LambdaFunction
```

Notice that you can use the ```!Sub``` for putting in specific variables.

```bash
sam build
```

```bash
sam deploy --guided
```

```bash
Configuring SAM deploy
======================

        Looking for config file [samconfig.toml] :  Found
        Reading default arguments  :  Success

        Setting default arguments for 'sam deploy'
        =========================================
        Stack Name [kinesissam-202103]: 
        AWS Region [ap-northeast-1]: 
        #Shows you resources changes to be deployed and require a 'Y' to initiate deploy
        Confirm changes before deploy [Y/n]: Y
        #SAM needs permission to be able to create roles to connect to the resources in your template
        Allow SAM CLI IAM role creation [Y/n]: Y
        Save arguments to configuration file [Y/n]: Y
        SAM configuration file [samconfig.toml]: 
        SAM configuration environment [default]: 

        Looking for resources needed for deployment: Found!

                Managed S3 bucket: aws-sam-cli-managed-default-samclisourcebucket-17xuw663c2f5p
                A different default S3 bucket can be set in samconfig.toml

        Saved arguments to config file
        Running 'sam deploy' for future deployments will use the parameters saved above.
        The above parameters can be changed by modifying samconfig.toml
        Learn more about samconfig.toml syntax at 
        https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-config.html

Uploading to kinesissam-202103/a12a2d5cebc8af1872b5b385439897e9  8048148 / 8048148  (100.00%)

        Deploying with following values
        ===============================
        Stack name                   : kinesissam-202103
        Region                       : ap-northeast-1
        Confirm changeset            : True
        Deployment s3 bucket         : aws-sam-cli-managed-default-samclisourcebucket-17xuw663c2f5p
        Capabilities                 : ["CAPABILITY_IAM"]
        Parameter overrides          : {}
        Signing Profiles             : {}

Initiating deployment
=====================
Uploading to kinesissam-202103/c560ae122f79e5a840cf04e347f2a975.template  680 / 680  (100.00%)

Waiting for changeset to be created..

CloudFormation stack changeset
-----------------------------------------------------------------------------------------------------------------------------------------
Operation                                                    LogicalResourceId                                            ResourceType                                                 Replacement                                                
-----------------------------------------------------------------------------------------------------------------------------------------
+ Add                                                        LambdaFunction                                               AWS::Lambda::Function                                        N/A                                                        
-----------------------------------------------------------------------------------------------------------------------------------------

Changeset created successfully. arn:aws:cloudformation:ap-northeast-1:879654127886:changeSet/samcli-deploy1615763991/b02090ec-954c-4f44-a769-18ec97c94c95


Previewing CloudFormation changeset before deployment
======================================================
Deploy this changeset? [y/N]: y

2021-03-14 23:20:02 - Waiting for stack create/update to complete

CloudFormation events from changeset
-----------------------------------------------------------------------------------------------------------------------------------------
ResourceStatus                                               ResourceType                                                 LogicalResourceId                                            ResourceStatusReason                                       
-----------------------------------------------------------------------------------------------------------------------------------------
CREATE_IN_PROGRESS                                           AWS::Lambda::Function                                        LambdaFunction                                               -                                                          
CREATE_IN_PROGRESS                                           AWS::Lambda::Function                                        LambdaFunction                                               Resource creation Initiated                                
CREATE_COMPLETE                                              AWS::Lambda::Function                                        LambdaFunction                                               -                                                          
CREATE_COMPLETE                                              AWS::CloudFormation::Stack                                   kinesissam-202103                                            -                                                          
-----------------------------------------------------------------------------------------------------------------------------------------

CloudFormation outputs from deployed stack
-----------------------------------------------------------------------------------------------------------------------------------------
Outputs                                                                                                                                                                                                                                         
-----------------------------------------------------------------------------------------------------------------------------------------
Key                 FunctionName                                                                                                                                                                                                                
Description         Function name                                                                                                                                                                                                               
Value               kinesissam-202103-LambdaFunction-XXXXXXXXX                                                                                                                                                                               
-----------------------------------------------------------------------------------------------------------------------------------------

Successfully created/updated stack - kinesissam-202103 in ap-northeast-1

```

Lets test the function. Go to the lambda console, and click on the newly created function through SAM.

![SQS](/images/2/35.png?width=90pc)

Open the **LambdaFunctionHandler** class. It looks like this:

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

3. Open the **Kinesis console** in the browser and select **Go to the Streams console**

![SQS](/images/2/20.png?width=90pc)

4.  You will see two streams - both have been provisioned by the lab template during setup. Click on **iDevelopDrawingData**

![SQS](/images/2/21.png?width=90pc)

5. Select the **Monitoring** tab and scroll down to reveal the **Incoming data - sum (Count)** graph.

![SQS](/images/2/22.png?width=90pc)

Around this location will be other statistics about the stream. Notice that the **Incoming data - sum (Count)** matches the same number you see in our demo website, under **Messages Sent**? You may need to move your mouse over the graph to see the actual data volume. In the example shown here, you would point your mouse at the end of the line to reveal the count (showing 118). The count of the two points on the graph will equal the **Messages Sent** metric on our website (last image, showing the Messages Sent from our website. 118)

![SQS](/images/2/23.png?width=90pc)

![SQS](/images/2/25.png?width=90pc)

1. When the Lambda function has finished uploading, open the **AWS Lambda console** in the browser. You will see the new Lambda function you just uploaded, (`kinesissam-202103-LambdaFunction-XXXXXXXXX`). Click on it to manage its configuration.
2. Under the Designer dashboard, you will find the **Add triggers** section.

![SQS](/images/2/26.png?width=90pc)

8. Click **Kinesis** from the list of triggers. It opens up Trigger configuration.
9. For **Kinesis stream** select ```iDevelopDrawingData``` from the list
10. For **Starting position** select ```Trim horizon```. Leave the **Batch size** at ```100```.

![SQS](/images/2/27.png?width=90pc)

11. Click **Add** and then click **Save**. The Kinesis trigger will be enabled for this Lambda function.

![SQS](/images/2/28.png?width=90pc)

12. It will take a moment or two for the Lambda function to be attached to the Kinesis stream. Wait a minute, and then click the **Refresh** icon on the **Kinesis** pannel. When the Lambda function is wired-up, you will see a message saying *Last processing result: OK*.

![SQS](/images/2/29.png?width=90pc)

13. Click the **Monitoring** tab. The draw points that have been sent into the Kinesis stream will have been consumed by the Lambda function, and you will see CloudWatch metrics indicating this activity. You will see activity in the *Invocation count* and *Invocation duration graphs*. Check that you don’t see any *Invocation error*s.

{{% notice note %}}
Depending on how many draw points you emited into the Kinesis stream, you will see a different number for *Invocation count*. When we enabled the trigger, we set the Batch size to 100 (which was the default). So the Lambda function will be invoked with up to 100 records at a time. If you sent 225 messages into the stream, this will be consumed by 3 invocations of the Lambda function - Math.ceil(225/100) == 3.
{{% /notice %}}

![SQS](/images/2/30.png?width=90pc)

14. Click the **View logs in CloudWatch** button to open the CloudWatch Log Stream for this Lambda function. Click on the first log stream in the list and review the logs listed:

![SQS](/images/2/31.png?width=90pc)

15. Scroll through the events in the CloudWatch Log output. These events have been written to the log by our Lambda function code:

![SQS](/images/2/32.png?width=90pc)

When writing your Lambda function code in Java, you can emit log strings into CloudWatch logs by simply calling the Logger’s .log() method:

```bash
context.getLogger().log("Log message to emit: " + someMessage);
```

#### Update the Lambda function implementation to publish received events to an SQS queue
Getting back to our scenario, we want to read the messages out of the Kinesis stream, and publish them into an SQS queue, where our browser implementation can read them and draw them on the subscriber’s canvas.

16. In the **Cloud9 IDE**, replace the **entire contents** of the ```kinesis_setup/src/main/java/com/amazonaws/lambda/demo/LambdaFunctionHandler.java``` file with the following code:

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

17. To recompile in SAM, **save** the new lambdafunction. On the terminal, run:

```bash
sam build
sam deploy
```

18. The Lambda function implementation expects the SQS Queue Url to be passed in via an Environment Variable. Before the function will be able to send messages to the SQS queue, you need to configure this variable.

Once the upload is complete, on the AWS Lambda console, click the **Configuration** tab for the `kinesissam-202103-LambdaFunction-XXXXXXXXX` Lambda function.\
Scroll down to the **Environment variables** section (if you don’t see it, click the name of your Lambda function in the Designer dashboard) and add a new variable with the key ```TargetSQSUrl``` and set the value to the ```QueueUrl1``` from the lab **Cloud Formation**'s **Output Results** tab. \
Then click **Save**.

You can use the aws cli to get these details if you are using the same terminal as before (note you have to have the environment ```$dets``` set)

```bash
aws cloudformation describe-stacks --stack-name $dets --query "Stacks[].Outputs[?OutputKey=='QueueUrl1'][OutputValue]" --output text
```

![SQS](/images/2/33.png?width=90pc)

1.  Now go back to our demo website and draw a new shape to emit events into the Kinesis stream.

![SQS](/images/2/34.png?width=90pc)

The drawing points should now be received via the SQS queue, and rendered on the canvas. Notice that the shape you draw is rendered inversed in the Y-dimension? This is because the implementation we used for the Lambda function executes a transform on the drawing points, just to demonstrate how we can manipulate the data in the Kinesis stream before passing it to the SQS queue.

Take a few moments to review the handler implementation code to ensure you understand how it works.
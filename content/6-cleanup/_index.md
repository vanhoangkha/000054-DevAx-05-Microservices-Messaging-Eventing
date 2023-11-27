+++
title = "Clean up resources"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++
You clean up resources in the following order:


#### Terminate EC2 Instance
1. Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* On the left navigation bar, click **Intances**.
* Select the instances we created in this lab. 
* Click **Instance state**
* Click **Terminate instance**
![Clean up reources](/images/6-cleanup/cleanup-001.png?featherlight=false&width=90pc)
2. Click **Terminate**
![Clean up reources](/images/6-cleanup/cleanup-002.png?featherlight=false&width=90pc)

#### Delete Users
1. Go to [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Type ```awsstudent``` to the search bar
* Select **awsstudent**. 
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-003.png?featherlight=false&width=90pc)
2. Type ```awsstudent``` to confirm, then click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-004.png?featherlight=false&width=90pc)

#### Delete CloudFormation Stack
1. Go to [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Select **DevAx-05**.
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-005.png?featherlight=false&width=90pc)
2. Click **Delete stack**
![Clean up reources](/images/6-cleanup/cleanup-006.png?featherlight=false&width=90pc)

#### Delete Lambda function
1. Go to [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home).
* Click **Functions**.
* Select **Kinesissam-LambdaFunction**.
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-007.png?featherlight=false&width=90pc)
2. Type ```delete``` to confirm, then click **Delete** to delete
![Clean up reources](/images/6-cleanup/cleanup-008.png?featherlight=false&width=90pc)

#### Delete CodeCommit Repositories
1. Go to [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Select **KinesisProducer**
* Click **Delete repository**
![Clean up reources](/images/6-cleanup/cleanup-009.png?featherlight=false&width=90pc)
2. Type ```delete``` to confirm, then click **Delete** to delete
![Clean up reources](/images/6-cleanup/cleanup-010.png?featherlight=false&width=90pc)
3. Do the same for the other CodeCommit Repository

#### Delete S3 bucket
1. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/).
* Click **Buckets**
* Select first S3 bucket.
* Click **Empty**.
![Clean up reources](/images/6-cleanup/cleanup-011.png?featherlight=false&width=90pc)
2. Type ```permanently delete``` to confirm, then click **Empty** to delete the data of this S3 bucket.
![Clean up reources](/images/6-cleanup/cleanup-012.png?featherlight=false&width=90pc)
3. Click **Exit** to back to the S3 inteface
![Clean up reources](/images/6-cleanup/cleanup-013.png?featherlight=false&width=90pc)
4. Click **Delete**.
![Clean up reources](/images/6-cleanup/cleanup-014.png?featherlight=false&width=90pc)
5. Type the name of the bucket then click **Delete bucket** to delete S3 bucket.
![Clean up reources](/images/6-cleanup/cleanup-015.png?featherlight=false&width=90pc)
6. Do the same for the other S3 buckets

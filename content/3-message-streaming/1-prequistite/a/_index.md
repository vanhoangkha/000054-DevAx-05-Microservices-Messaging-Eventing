+++
title = "Eclipse Environment"
date = 2021
weight = 1
chapter = false
pre = "<b>Option A. </b>"
+++

Our example use case simulates sales data coming from the field, and being processed via a Kinesis stream and written out into ElasticSearch where we can analyse the sales data graphically with Kibana. The sales data will be read from a CSV file, and have costs, quantities and transaction dates randomly applied. The architecture looks like this:

![SQS](/images/3/kinesisconsumerproducerapparch.png?width=50pc)

**Contents**
- [Clone the repository in your development environment using the Eclipse IDE](#clone-the-repository-in-your-development-environment-using-the-eclipse-ide)
- [Clone the repository on the Kinesis Consumer EC2 instance](#clone-the-repository-on-the-kinesis-consumer-ec2-instance)
- [Create a new repository for the Kinesis Producer source code](#create-a-new-repository-for-the-kinesis-producer-source-code)
- [Clone the repository in your development environment using the Eclipse IDE](#clone-the-repository-in-your-development-environment-using-the-eclipse-ide-1)

1. Open the **AWS CodeCommit console** in the browser.
2. On the landing page, click **Get Started**
3. On the **Create repository** page, for **Repository name** type ```KinesisConsumer```
4. For **Description** type ```Sample application that consumes messages from a Kinesis stream and writes them to an ElasticSearch domain index```.

![MessageStreaming](/images/3/1.png?width=90pc)

5. Click **Create repository**. Your new repository will be created, ready for you to use. Click **Skip** to close the next screen. Click **Close** to close the panel that appears after the repository is created.

#### Clone the repository in your development environment using the Eclipse IDE
6. In the **Eclipse IDE**, open the **Git Perspective**. If you don’t already have a shortcut to this perspective, use the **Window | Perspective | Open Perspective | Git repositories** menu option.
7. Click the **Clone repository** icon (tooltip: *Clone a repository and add the clone to this view*)

![MessageStreaming](/images/3/2.png?width=90pc)

8. In the AWS CodeCommit console in the browser, click the **Clone URL** button. Select **HTTPS** from the menu and then copy the URL into your clipboard.

![MessageStreaming](/images/3/3.png?width=90pc)

9. Back in the Eclipse IDE, paste the clone URL into the **URI** field:

![MessageStreaming](/images/3/4.png?width=90pc)

10. In the **Authentication** section, for **User** paste in the GitUsername value from value from **Cloudformation Output** tab.
11. For the **Password** field, paste in the **GitPassword** value from value from **Cloudformation** Output tab.

![MessageStreaming](/images/3/5.png?width=90pc)

12. Click **Next**
13. In **Branch Selection**, there are no branches created, so just click **Next**.
14. In the **Local destination** panel, select a suitable path on your development filesystem and click **Finish:**

![MessageStreaming](/images/3/6.png?width=90pc)

The empty repository will be cloned into the local filesystem path you specified.

15. Download the source code bundle from [**KinesisConsumer.zip**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/KinesisConsumer.zip) and explode the zip file onto your filesystem.
16. Copy the contents of the exploded zip file across to the filesystem location where you have cloned the KinesisConsumer repository.
17. In the **Eclipse IDE**, switch to the Java perspective and open the project from the filesystem by using the File | Open projects from filesystem… menu option.
18. In the **Eclipse IDE**, open the **config.java** file.

![MessageStreaming](/images/3/7.png?width=90pc)

19. Update the value of the `public static final String ELASTICSEARCH_ENDPOINT` to match the value in value from **Cloudformation Output** tab, called **ElasticsearchEndpoint**. By default, the value is set to **REPLACE_WITH_YOUR_ELASTIC_SEARCH_ENDPOINT**. Erase this value and replace it with your **ElasticsearchEndpoint** value. Save the file.
20. Open a terminal window and ```cd``` into the folder where you cloned the repository.
21. Create a new master branch by issuing the command:

```bash
git checkout -b master
```

22. Add the new files to the repository by issuing the command:

```bash
git add .
```

23. Commit the new files to the branch by issuing the following command:

```bash
git commit -m "Initial commit"
```

24. In the Eclipse IDE git perspective, right-click on the *KinesisConsumer* repository, and select **Push Branch Master**

{{% notice note %}}
If this option does not appear, or the branch **Master** is not present, you may need to use the refresh action to update the IDE.
{{% /notice %}}

25. Click **Finish**. The branch will be pushed to the CodeCommit repository. Confirm this by refreshing the CodeCommit console in the web browser:

![MessageStreaming](/images/3/8.png?width=90pc)

You should see the files that you have added into the repository listed in the browser view.

#### Clone the repository on the Kinesis Consumer EC2 instance
The labs setup has provisioned an EC2 instance for you to run as the Consumer in this scenario. You will now connect to the EC2 instance, clone the repository, build and run the Consumer code.

26. In order to securely connect into your cloud infrastructure, you will use SSH, the secure shell. SSH requires a public/private key-pair to be set up and installed on the client and server that will communicate via the SSH protocol. The EC2 instances have been launched with a public key. You need to retrieve the private key so you can use that to establish a secure connection. Click **Download PEM**. Save this private key file on your local machine.
27. In a terminal session on your local machine, run the following command. This command will ensure you can use the downloaded private key to log into the EC2 instances you will be using.

```bash
chmod 400 <path to your downloaded PEM file>
```

Example:
```bash
chmod 400 ~/Downloads/ee-default-keypair.pem
```
{{% notice note %}}
Leave the terminal session open, you will return shortly.
{{% /notice %}}

28. Inside AWS Console, Cloudformation page, in the Outputs tab, locate the **EC2InstanceKinesisConsumer** value and copy it to your clipboard.

![MessageStreaming](/images/3/9.png?width=90pc)

29. Use the command below as a template to connect to the instance:
```bash
ssh -i <path to your downloaded PEM file> ec2-user@<EC2InstanceKinesisConsumer>
```
Example:
```bash
ssh -i ~/Downloads/ee-default-keypair.pem ec2-user@34.212.21.7
```
{{% notice warning %}}
Don’t include the **<** and **>** symbols. If you have difficulty with this step please seek help from a lab assistant. Once you have successfully logged on to the instance via SSH, continue below.
{{% /notice %}}


30. On the terminal window you used to SSH to the instance, issue this command to clone the repository:
```bash
git clone https://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/KinesisConsumer
```
You will be asked for username and password. You can obtain these from **Cloudformation Outputs** tab - look for **GitUsername** and **GitPassword**.

31. The source code you had checked in from your workstation will now be cloned onto the EC2 instance and will take a moment to complete.
32. The setup process has installed Maven on the EC2 instance so you can build the source code. cd into the KinesisConsumer directory and execute this command:
```bash
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
It will take a few moments to complete the build.

33. When the build is complete, start the Kinesis Consumer application by issuing:
```bash
java -jar target/KinesisConsumerApp-1.0.0.jar
```
The application will start and wait for events to be published into the Kinesis stream. You should see no errors, and after a few moments, the console output should show ‘Sleeping…’ since there are no records in the stream to process:
```
INFO: Initializing record processor for shard: shardId-000000000000
Aug 21, 2017 11:30:19 AM idevelop.demo.RecordProcessor initialize
INFO: Initializing record processor for shard: shardId-000000000001
Aug 21, 2017 11:30:57 AM com.amazonaws.services.kinesis.clientlibrary.lib.worker.Worker info
INFO: Current stream shard assignments: shardId-000000000001, shardId-000000000000
Aug 21, 2017 11:30:57 AM com.amazonaws.services.kinesis.clientlibrary.lib.worker.Worker info
INFO: Sleeping ...
```
Stop the execution in the terminal window by issuing `CTRL + C`

#### Create a new repository for the Kinesis Producer source code
	You will now repeat the process above, this time, creating a repository for the Kinesis Producer application, and cloning it on to the Kinesis Producer EC2 instance. Once this is complete, you can begin testing data transfer between the two machines, via Amazon Kinesis.

	The process of creating the repository is the same as the previous steps, but we repeat it here for convenience.

34. Open the **AWS CodeCommit console** in the browser.
35. On the **Create repository** page, for **Repository name** type ```KinesisProducer```
36. For **Description** type ```Sample application that publishes sales events into a Kinesis stream```.
37. Click **Create repository**. Your new repository will be created, ready for you to use. Click **Skip** in the next step. You can close the panel that appears after the repository is created.

#### Clone the repository in your development environment using the Eclipse IDE
38. In the Eclipse IDE, open the **Git Perspective**. If you don’t already have a shortcut to this perspective, use the **Window | Perspective | Open Perspective** menu option.
39. Click the **Clone repository** icon (tooltip: *Clone a repository and add the clone to this view*)
40. In the **AWS CodeCommit console** in the browser, click the **Clone URL** button. Copy the **clone URL for HTTPS** into your clipboard.
41. Back in the **Eclipse IDE**, paste the clone URL into the **URI** field.
42. The credentials you used in the previous steps should still be populated in the **Authentication** section - but if not, for **User** paste in the **GitUsername** value from **Cloudformation Outputs** tab. For the **Password** field, paste in the **GitPassword** value from **Cloudformation Output** tab.
43. Click **Next**
44. In **Branch Selection**, there are no branches created, so just click **Next**.
45. In the **Local destination** panel, select a suitable path on your development filesystem and click **Finish:**
	
	The empty repository will be cloned into the local filesystem path you specified.

46. Download the source code bundle from [**KinesisProducer.zip**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/KinesisProducer.zip) and explode the zip file onto your filesystem.
47. Copy the contents of the exploded zip file across to the filesystem location where you have cloned the KinesisProducer repository.
48. Open a terminal window and ```cd``` into the folder where you cloned the repository.
49. Create a new master branch by issuing the command:
```bash
git checkout -b master
```
50. Add the new files to the repository by issuing the command:
```bash
git add .
```
51. Commit the new files to the branch by issuing the following command:
```bash
git commit -m "Initial commit"
```
52. In the **Eclipse IDE** git perspective, right-click on the *KinesisProducer* repository, and select **Push Branch Master**

![MessageStreaming](/images/3/10a.png?width=90pc)

53. Click **Next** and on the next page, click **Finish**.

The branch will be pushed to the CodeCommit repository. Confirm this by refreshing the CodeCommit console in the web browser. You should see the files that you have added into the repository listed in the browser view.

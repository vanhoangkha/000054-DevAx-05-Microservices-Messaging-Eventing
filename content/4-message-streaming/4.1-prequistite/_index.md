+++
title = "Preparation"
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++
#### Preparation
Our example use case simulates sales data coming from the field, and being processed via a Kinesis stream and written out into ElasticSearch where we can analyse the sales data graphically with Kibana. The sales data will be read from a CSV file, and have costs, quantities and transaction dates randomly applied. The architecture looks like this:
![Prequistite](/images/4-message-streaming/4.1-prequistite/kinesisconsumerproducerapparch.png?featherlight=false&width=70pc)

#### Create a new repository for Kinesis Consumer source

1. Go to [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Click **Create repository**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-001.png?featherlight=false&width=90pc)
2. In the **Create repository** page
* In the **Repository name** section, type ```KinesisConsumer```
* In the **Description** section, type ```Sample application that consumes messages from a Kinesis stream and writes them to an ElasticSearch domain index```
* Click **Create**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-002.png?featherlight=false&width=90pc)
3. In the **KinesisConsumer** page, click **Clone URL**
* Click **Clone HTTPS** to save the url
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-006.png?featherlight=false&width=90pc)
4. In the **Eclipse IDE**, click **Window**
* Click **Perspective**
* Click **Open Perspective**
* Click **Other**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-003.png?featherlight=false&width=90pc)
5. In the **Open Perspective** section, click **Git**
* Click **Open**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-004.png?featherlight=false&width=90pc)
6. Click **Clone a Git repository**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-005.png?featherlight=false&width=90pc)
7. In the **Source Git Repository** section
* In the **URI** section, type the url we saved in step 3
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-007.png?featherlight=false&width=90pc)
8. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Type ```DevAx-05``` to the search bar, press **Enter**
* Click **DevAx-05**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-008.png?featherlight=false&width=90pc)
9. Click tab **Output**
* Save the value of **GitUserName** and **GitPassword**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-009.png?featherlight=false&width=90pc)
10. In the **Eclipse IDE**, type the value of **User** and **Password** we saved in step 9
* Click **Next**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-010.png?featherlight=false&width=90pc)
11. Click **Next**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-011.png?featherlight=false&width=90pc)
12. Click **Finish**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-012.png?featherlight=false&width=90pc)

{{%attachments title="KinesisConsumer Project" style="orange" pattern="KinesisConsumer.zip"/%}}

13. Download file **KinesisConsumer.zip** and extract
* Copy the contents of the exploded zip file across to the filesystem location where you have cloned the **KinesisConsumer** repository.
14. In the **Eclipse IDE**, Click **File**
* Click **Open projects from filesystem…**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-013.png?featherlight=false&width=90pc)
15. Choose the path of the **KinesisConsumer** project
* Click **Finish**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-014.png?featherlight=false&width=90pc)
16. Open file **KinesisConsumer\src\main\java\idevelop\demo\config.java**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-015.png?featherlight=false&width=90pc)
17. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Type ```DevAx-05``` to the search bar, press **Enter**
* Click **DevAx-05**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-016.png?featherlight=false&width=90pc)
18. Click tab **Output**
* Save the value of **ElasticsearchEndpoint**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-017.png?featherlight=false&width=90pc)
19. In the **Eclipse IDE**, replace **REPLACE_WITH_YOUR_ELASTIC_SEARCH_ENDPOINT** by **ElasticsearchEndpoint** we saved in step 18
* In the **REGION** section, replace **ap-southeast-2** by your **Region code**
* Save
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-018.png?featherlight=false&width=90pc)
20. Open **Command Prompt** and navigate to the directory of **KinesisConsumer** project
* Execute the below command to create **master** branch
```
git checkout -b master
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-019.png?featherlight=false&width=60pc)
21. Execute the below command to add the new file
```
git add .
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-020.png?featherlight=false&width=60pc)
22. Execute the below command to commit the new file
```
git commit -m "Initial commit"
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-021.png?featherlight=false&width=60pc)
23. In the **Eclipse IDE**, right-click on **KinesisConsumer** repository
* Click **Push Branch \`master\`...**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-022.png?featherlight=false&width=90pc)
24. Type the value of **User** and **Password** we saved in step 9 to **User** section and **Password** section
* Click **Login**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-023.png?featherlight=false&width=90pc)
25. Click **Preview**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-024.png?featherlight=false&width=90pc)
26. Click **Push**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-025.png?featherlight=false&width=90pc)
27. Go to [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Type ```KinesisConsumer``` to the search bar
* Click **KinesisConsumer**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-026.png?featherlight=false&width=90pc)
28. You will see the files that you have added into the repository listed in the browser view.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-027.png?featherlight=false&width=90pc)

#### Clone the repository on the Kinesis Consumer EC2 instance

The labs setup has provisioned an EC2 instance for you to run as the Consumer in this scenario. You will now connect to the EC2 instance, clone the repository, build and run the Consumer code.

{{% notice note %}} 
In order to securely connect into your cloud infrastructure, you will use SSH, the secure shell. SSH requires a public/private key-pair to be set up and installed on the client and server that will communicate via the SSH protocol. The EC2 instances have been launched with a public key. You need to retrieve the private key so you can use that to establish a secure connection. Click Download PEM. Save this private key file on your local machine.
{{% /notice %}}

29. Open **Command Prompt** and execute the below command
```
chmod 400 <path to your key-pair file>
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-028.png?featherlight=false&width=60pc)
30. Execute the below command
```
ssh -i <path to your key-pair file> ec2-user@<EC2InstanceKinesisConsumer>
```
{{% notice note %}} 
Replace **\<EC2InstanceKinesisConsumer\>** by the value of **EC2InstanceKinesisConsumer** in the **Output** tab of **DevAx-05** stack
{{% /notice %}}
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-029.png?featherlight=false&width=60pc)
31. Execute the below command
```
git clone https://git-codecommit.<your_region>.amazonaws.com/v1/repos/KinesisConsumer
```
{{% notice note %}} 
Replace **<your_region>** by your **Region**
{{% /notice %}}
* Type **User** and **Password** we saved in step 9 to **Username** section and **Password** section
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-030.png?featherlight=false&width=60pc)
32. Execute the below command
```
cd KinesisConsumer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-033.png?featherlight=false&width=60pc)
33. It will take a few moments to complete the build.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-034.png?featherlight=false&width=60pc)
34. When the build is complete, start the Kinesis Consumer application by executing the below command
```
java -jar target/KinesisConsumerApp-1.0.0.jar
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-035.png?featherlight=false&width=60pc)
The application will start and wait for events to be published into the Kinesis stream. You should see no errors, and after a few moments, the console output should show **Sleeping…** since there are no records in the stream to process:
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-036.png?featherlight=false&width=60pc)

#### Create a new repository for the Kinesis Producer source code
You will now repeat the process above, this time, creating a repository for the **Kinesis Producer** application, and cloning it on to the Kinesis Producer EC2 instance. Once this is complete, you can begin testing data transfer between the two machines, via Amazon Kinesis.

The process of creating the repository is the same as the previous steps, but we repeat it here for convenience.

35.  Go to [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Click **Create repository**
36. In the **Create repository** page
* In the **Repository name** section, type ```KinesisProducer```
* In the **Description** section, type ```Sample application that publishes sales events into a Kinesis stream```
* Click **Create**
37. In the **KinesisProducer** page, click **Clone URL**
* Click **Clone HTTPS** to save the url
38. In the **Eclipse IDE**, click **Clone a Git Repository and add the clone to this view** icon
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037.png?featherlight=false&width=90pc)
39. In the **Source Git Repository** section
* In the **URI** section, type url we saved in step 37
40. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Type ```DevAx-05``` to the search bar, press **Enter**
* Click **DevAx-05**
41. Click tab **Output**
* Save the value of **GitUserName** and **GitPassword**
42. In the **Eclipse IDE**, type the value of **User** and **Password** we saved in step 9
* Click **Next**
43. Click **Next**
44. Click **Finish**

{{%attachments title="KinesisProducer Project" style="orange" pattern="KinesisProducer.zip"/%}}

45. Download file **KinesisProducer.zip** and extract
* Copy the contents of the exploded zip file across to the filesystem location where you have cloned the **KinesisProducer** repository.
46. In the **Eclipse IDE**, Click **File**
* Click **Open projects from filesystem…**
47. Select the path of **KinesisProducer** project
* Click **Finish**
48. Open file **KinesisProducer\src\main\java\idevelop\demo\KinesisProducerApp.java**
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037a.png?featherlight=false&width=90pc)
49. In the line 69, replace **AP_SOUTHEAST_2** by your **Region**
* Save
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-037b.png?featherlight=false&width=90pc)
50. Open **Command Prompt** and navigate to the directory of **KinesisProducer**
* Execute the below command to create **master** branch
```
git checkout -b master
```
51. Execute the below command to add the new files
```
git add .
```
52. Execute the below command to commit the new files
```
git commit -m "Initial commit"
```
53. In the **Eclipse IDE**, right-click on **KinesisProducer** repository
* Click **Push Branch \`master\`...**
54. Type the value of **User** and **Password** we saved in step 41 to **User** section and **Password** section
* Click **Login**
55. Click **Preview**
56. Click **Push**
57. Go to [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Type ```KinesisProducer``` to the search bar
* Click **KinesisProducer**
58. You will see the files that you have added into the repository listed in the browser view.

#### Clone the repository on the Kinesis Producer EC2 instance

59. Open **Command Prompt** and execute the below command
```
chmod 400 <path to your key-pair file>
```
60. Execute the below command
```
ssh -i <path to your key-pair file> ec2-user@<EC2InstanceKinesisProducer>
```
{{% notice note %}} 
Replace **\<EC2InstanceKinesisProducer\>** by the value of **EC2InstanceKinesisProducer** in the **Output** tab of the **DevAx-05** stack
{{% /notice %}}
61. Execute the below command
```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
git clone https://git-codecommit.<your_region>.amazonaws.com/v1/repos/KinesisProducer
```
{{% notice note %}} 
Replace **<your_region>** by your **Region**
{{% /notice %}}
* Type **User** and **Password** we saved in step 41 to **Username** section and **Password** section
62. Execute the below command
```
cd KinesisProducer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```
63. It will take a few moments to complete the build.
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-039.png?featherlight=false&width=60pc)
64. The Producer needs source data, and is expecting a CSV file in the current working folder, called **SalesData.csv**.
* Execute the below command to copy this file to EC2 instance
```
wget https://workshops.devax.academy/monoliths-to-microservices/module5/files/SalesData.csv
```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-040.png?featherlight=false&width=60pc)

#### Configure ElasticSearch index mappings

When data is sent to ElasticSearch, it tries to infer the data types in the content, and usually does a good job of getting the data types right. In our case, we are sending the date/time of each of the sales record as **long** representation (ticks since Epoch) and so ElasticSearch would interpret this field as just a number (of type long). Instead, we need to warn ElasticSearch that it should interpret this timestamp field as a **Date**. We do this by creating Mappings in the index before we send any records.
65. On the terminal window you have the SSH connection to the **Kinesis Consumer EC2 instance**, Execute the below command
```
curl -X PUT "https://<ElasticsearchEndpoint>/transactions/" -k -d '{
	"mappings": {
		"transaction": {
			"properties": {
				"timestamp": {
					"type": "date"
				},
				"productLine": {
					"type": "string"
				},
				"retailerType": {
					"type": "string"
				},
				"productType": {
					"type": "string"
				},
				"product": {
					"type": "string"
				},
				"quantity": {
					"type": "long"
				},
				"unitCost": {
					"type": "double"
				},
				"totalCost": {
					"type": "double"
				}
			}
		}
	}
}';
```
66. If successful, you will see the message ```{“acknowledged”:true}```
![Prequistite](/images/4-message-streaming/4.1-prequistite/prequistite-041.png?featherlight=false&width=60pc)
+++
title = "AWS Cloud9 Environment"
date = 2021
weight = 2
chapter = false
pre = "<b>Option B. </b>"
+++

The lab setup has provisioned an EC2 instance for you to run as the Producer in this scenario. You will now connect to the EC2 instance, clone the repository, build and run the Producer code. Note that this process is identical to the one you just completed on the Consumer EC2 instance, but we repeat the instructions here for convenience.

**Contents**
- [Setup Kinesis Producer](#setup-kinesis-producer)
- [Configure ElasticSearch index mappings](#configure-elasticsearch-index-mappings)

#### Setup Kinesis Producer

1. Inside **AWS Console**, **Cloudformation** page, in the **Outputs** tab, locate the **EC2InstanceKinesisProducer** value and copy it to your clipboard.

![ElasticSearch](/images/3/11.png?width=90pc)

2. Using a new terminal window, issue the command below as a template to connect to the instance:
```bash
ssh -i <path to your downloaded PEM file> ec2-user@<EC2InstanceKinesisProducer>
```

**Example:**
```bash
ssh -i ~/Downloads/ee-default-keypair.pem ec2-user@34.212.21.7
```

3. Once connected via SSH, issue this command to clone the repository:

You need to setup the helper classes to get this working as before:
```bash
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
git clone https://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/KinesisProducer
```

4. The source code you had checked in from your workstation will now be cloned onto the EC2 instance and will take a moment to complete.
5. Build the source code using these commands:
```bash
cd KinesisProducer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```

It will take a few moments to complete the build.

6. The Producer needs source data, and is expecting a CSV file in the current working folder, called SalesData.csv. Copy the file onto the EC2 instance by using the ```wget``` or ```curl``` command and specifying the file URL. You can get the file URL by right-clicking on the following file and copying its link address: [**SalesData.csv**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/SalesData.csv).

#### Configure ElasticSearch index mappings

When data is sent to ElasticSearch, it tries to infer the data types in the content, and usually does a good job of getting the data types right. In our case, we are sending the date/time of each of the sales record as **long** representation (ticks since Epoch) and so ElasticSearch would interpret this field as just a number (of type long). Instead, we need to warn ElasticSearch that it should interpret this timestamp field as a **Date**. We do this by creating **Mappings** in the index before we send any records.

7. On the terminal window you have the SSH connection to the **Kinesis Consumer** EC2 instance, issue the following command - but replace with the ***ElasticsearchEndpoint*** value from the labs **Additional Info** panel:
```bash
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

If successful, you will see a message.

```json
{“acknowledged”:true}
```

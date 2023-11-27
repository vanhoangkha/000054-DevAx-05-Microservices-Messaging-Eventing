+++
title = "Confirm The Records In ElasticSearch"
date = 2020
weight = 3
chapter = false
pre = "<b>4.3. </b>"
+++
#### Confirm The Records In ElasticSearch
In this step, we will confirm that the sales data records are making their way to the ElasticSearch index correctly.

1. Go to [**Amazon ElasticSearch Service console**](https://console.aws.amazon.com/esv3/home).
* Click **Dashboard**
* Click **Views all domains**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-001.png?featherlight=false&width=90pc)
2. Under the **Searchable documents column**, you should see a number greater than zero here, if documents are being stored correctly.
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-002.png?featherlight=false&width=90pc)
3. Click **idevelop**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-003.png?featherlight=false&width=90pc)
4. In the **idevelop** page
* Click tab **Indices**
* Click **transactions**
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-004.png?featherlight=false&width=90pc)
5. You will see details of how many documents are indexed, approximate size of storage required, and the Mappings information that you set previously using the curl command to inform ElasticSearch of the data types you are using in your documents for this index:
![Confirm The Records In ElasticSearch](/images/4-message-streaming/4.3-verify-elasticsearch/verify-elasticsearch-005.png?featherlight=false&width=90pc)
+++
title = "Checking in ElasticSearch"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4. </b>"
+++

**Confirm the records in ElasticSearch**

In this step, we will confirm that the sales data records are making their way to the ElasticSearch index correctly.

1. Open the **Amazon ElasticSearch Service console** in the web browser
2. On the **Amazon Elasticsearch Service dashboard**, locate the **idevelop domain**. Under the **Searchable documents column**, you should see a number **greater than zero** here, if documents are being stored correctly.

![ElasticSearch](/images/3/12.png?width=90pc)

3. Click on the **idevelop** link to drill into the cluster.
4. Click the **Indices** tab to show the indices in the cluster. You should see details of how many documents are indexed, approximate size of storage required, and the **Mappings** information that you set previously using the ```curl``` command to inform ElasticSearch of the data types you are using in your documents for this index:

![ElasticSearch](/images/3/13.png?width=90pc)

{{% notice note %}}
If you don’t see any documents in the index, check you have followed all the instructions above, and that the Kinesis Consumer application is in fact showing feedback such as ***INFO: Processing xxxx records*** and not showing any errors. If you are stuck, ask a lab assistant for help.
{{% /notice %}}

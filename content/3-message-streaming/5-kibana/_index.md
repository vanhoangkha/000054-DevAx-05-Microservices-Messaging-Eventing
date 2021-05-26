+++
title = "Explore Kibana"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5. </b>"
+++

**Discover and Visualise the data in Kibana**

Amazon ElasticSearch comes with the Kibana plugin installed to assist you in visualising and analysing your data. Because of the way the security model works, all calls into the Elastic Search cluster, including connecting to Kibana, must be signed with credentials. For us to connect to Kibana we need to either sign the request which the browser can’t do or we need to open it to our IP. Let’s do the latter and add our IP to the cluster security policy.

1. Go to your **Amazon ElasticSearch Cluster** in the **AWS Console**.

2. Once you selected your cluster, go to **Actions** and **Modify access policy**.

3. You’ll see a JSON defined for you. Find the part with **Condition** and **IpAddress** and change it to the following (**X.X.X.X** reflect the previous value, please keep it):

```json
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": [
            "X.X.X.X",
            "<ADD YOUR PUBLIC IP OR RANGE>"
          ]
        }
      }
```

{{% notice tip %}}
You can either use https://checkip.amazonaws.com/ to identify your IP, or use some other tools like curl ifconfig.me
{{% /notice %}}

4. After you changed the Policy, you can click on the Kibana link in the overview tab.

5. You will see the Kibana loading page while things are setup - it will only take a moment.

![ElasticSearch](/images/3/14.png?width=90pc)

6. Your first task is to set up a default index pattern. In the field beneath **Index name or pattern**, replace the contents with an asterisk (*) as shown:

![ElasticSearch](/images/3/15.png?width=90pc)

Kibana will then automatically find the **timestamp** field in our transactions index. Click **Create** to confirm.

7. Click **Discover** in the top menu tab. You may not see any records rendered. At this point, Kibana is defaulting to only show you records that have a *timestamp* value in the range now minus 15 minutes ago. Our application generates sales events with timestamps starting in 2015 and advancing forward at random increments. Therefore, you may not see any records for the timeframe that spans the *Last 15 minutes*. Widen the timeframe by clicking on the **Last 15 minutes** link at the top right of the page, and in the list that appears, choose **Last 5 years**

![ElasticSearch](/images/3/16.png?width=90pc)

    You should now see much more data!

8. Click on one of the records in the list to expand it. The fields in the data relate to our transaction record for the fictitious sales - productLine, product, totalCost, etc.

![ElasticSearch](/images/3/17.png?width=90pc)

9. If you refer back to the Java code in the Eclipse IDE, in both the Producer and Consumer applications, there is a class called ***SaleItemTransaction*** which has these same fields in it. This class is used to serialise and deserialise the sales data into and out of the Kinesis stream, and further, into the ElasticSearch index as you can see here.

#### Visualise the data in Kibana
In this step, we will visualise and analyse the sales data using Kibana.

10. Click **Visualize** in the top menu.
11. Choose **Pie chart** from the list of available graphing options:

![ElasticSearch](/images/3/18.png?width=90pc)

12. In step 2, select **From a new search**

13. Initially, you will see a pie chart that is a full circle made up of only one slice. We need to filter our data. Click **Split Slices**.

14. For **Aggregation** select ```Terms```

15. For **Field** select ```productType```

16. For **Size** type ```5```

17. Click the **Play Icon** at the top of the left-side panel to re-render the graph:

![ElasticSearch](/images/3/19.png?width=90pc)

    Your dataset will be different (due to the randomly-selected transactions and their details) but you will see a result similar to that shown here. In this example, *Eyewear* is the top moving product in the time period selected (Last 5 years).

18. Click **Add sub-buckets**
19. Click **Split slices**
20. For **Sub aggregation** select ```Terms```
21. For **Field** select ```retailerType```
22. Click the **Play Icon** at the top of the left-side panel to re-render the graph:

![ElasticSearch](/images/3/20.png?width=90pc)

Again, your data will be different, but in this example, we can see the breakdown of the count of transactions at each of the retailer types, for each of the top selling product types.

As you can see, Kibana is a very powerful tool for analysing your data. Take some time to experiment with other graph types, fields and aggregations. You can save each visualisation you create, and then add them to a dashboard using the tools and icons in the Kibana web page.

{{% notice note %}}
If you need to clear the records, mappings and index from the ElasticSearch domain, you can issue the following from a terminal window:
{{% /notice %}}

```bash
curl -XDELETE https://<ElasticsearchEndpoint>/transactions -k
```

But replace with the ***ElasticsearchEndpoint*** value from from **Cloudformation Outputs** tab.

You will then need to re-create the mappings, by running the mappings command you ran earlier. You can then re-populate your data set by running the Consumer and Producer applications again.

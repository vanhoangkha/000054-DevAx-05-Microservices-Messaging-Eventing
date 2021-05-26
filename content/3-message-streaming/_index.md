+++
title = "Implementing Message Streaming Using Kinesis"
date = 2021
weight = 3
chapter = false
pre = "<b>3. </b>"
+++

When we used Kinesis in the browser to publish messages, the throughput to Kinesis was constrained by the threading model in the browser, and the fact that we were writing messages into the Kinesis stream one record at a time. The demonstration was designed to illustrate a simple use case and how to attach a Lambda consumer to a Kinesis stream, and not the scalability of the service. In this exercise, we will look at the scalability and throughput aspects.

You can scale Kinesis using shards. Each shard can support up to 5 transactions per second for reads, up to a maximum total data read rate of 2 MB per second. Each shard can also support up to 1,000 records per second for writes, up to a maximum total data write rate of 1 MB per second. This write limit applies to operations such as PutRecord and PutRecords.

You can submit up to 500 records in a single PutRecords call, and receive up to 10,000 records in a single GetRecords call.

The **Kinesis Client Library** takes care of the underlying mechanics of using Kinesis, keeping state in an Amazon DynamoDB table and managing the complexities of shards, shard iterators etc. In this exercise, we will run two Java programs, one that produces records and one that consumes records from a Kinesis stream. The consumer will write the records it receives into an **Amazon ElasticSearch** domain index so we can run queries on the data. The consumer makes use of the Kinesis Client Library to reduce the development effort.

We will also create two new AWS CodeCommit repositories to share our code between our development environment, and EC2 instances that will run the code in our test environment.

#### Contents
1. [Prerequisites](../3-message-streaming/1-prequistite/)
2. [Create the Kinesis Producer](../3-message-streaming/2-kinesis-producer/)
3. [Send records from the Producer to the Consumer](../3-message-streaming/3-send-records/)
4. [Confirm the records in ElasticSearch](../3-message-streaming/4-verify-elasticsearch/)
5. [Discover and Visualise the data in Kibana](../3-message-streaming/5-kibana/)

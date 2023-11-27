+++
title = "Introduction"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++
#### Introduction

In this lab you will explore the different options the AWS platform has for messaging. Messaging is an important concept to understand when designing scalable, decoupled architectures, making them more resilient to failure. Building applications from individual components that each perform a discrete function improves scalability and reliability, and is best practice design for modern applications. AWS has several messaging solutions, from queuing to streaming, and we will investigate the behaviours of each as we go through the lab.

#### Amazon SQS
**Amazon Simple Queue Service (SQS)** is a fully managed message queuing service that makes it easy to decouple and scale microservices, distributed systems and serverless applications. SQS makes it simple and cost-effective to decouple and coordinate the components of a cloud application. Using SQS, you can send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be always available. SQS standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent, with limited throughput.

SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

#### Amazon SNS
**Amazon Simple Notification Service (Amazon SNS)** is a flexible, fully managed pub/sub messaging and mobile notifications service for coordinating the delivery of messages to subscribing endpoints and clients. With SNS you can fan-out messages to a large number of subscribers, including distributed systems and services, and mobile devices. It is easy to set up, operate, and reliably send notifications to all your endpoints – at any scale.

#### Amazon Kinesis
**Amazon Kinesis** makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. With Amazon Kinesis, you can ingest real-time data such as application logs, website clickstreams, IoT telemetry data, and more into your databases, data lakes and data warehouses, or build your own real-time applications using this data.

#### Amazon IoT
**AWS IoT** is a managed cloud platform that lets connected devices easily and securely interact with cloud applications and other devices. AWS IoT can support billions of devices and trillions of messages, and can process and route those messages to AWS endpoints and to other devices reliably and securely. AWS IoT can also be used by your applications where you need an ephemeral pub/sub bus for fast, light-weight messaging between components.

#### Content
By the end of this lab, you will be able to:
* Understand the different behaviours of the various messaging options on the AWS platform
* Explain the differences between queuing and streaming data
* Use AWS messaging solutions from the web browser and programmatically using the Java SDK
* Understand how to read data from an Amazon Kinesis stream using an AWS Lambda function
* Create a pub/sub communication channel between two EC2 instances, via an Amazon Kinesis stream, using the AWS SDK for Java
* Learn how to create a git repository in AWS CodeCommit and access the repository in your development environment and EC2 instances

#### Technical Knowledge Prerequisites
To successfully complete this lab, you should be familiar with basic navigation of the AWS Management Console and have intermediate experience using the Eclipse IDE and the Java Programming language.

#### Environment
All the resources required to begin this lab have already been provisioned and set up for you. If running in your own account, use this CloudFormation template The following diagram depicts the resources that were deployed in your AWS account.
![Architecture](/images/1-introduction/architecture.png?featherlight=false&width=50pc)
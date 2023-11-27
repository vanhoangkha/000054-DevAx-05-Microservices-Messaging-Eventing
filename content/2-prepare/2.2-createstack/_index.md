+++
title = "Create a CloudFormation stack"
date = 2020
weight = 2
chapter = false
pre = "<b>2.2. </b>"
+++
#### Create a CloudFormation stack

{{%attachments title="Template File" style="orange" pattern="Module5.template.yaml"/%}}
1. Download file **Module5.template.yaml**.
2. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Click **Create stack**.
* Click **With new resources (standard)**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-001.png?featherlight=false&width=90pc)
3. In the **Specify template** section.
* Select **Upload a template file**
* Click **Choose file**, then select file **Module5.template.yaml** we downloaded.
* Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-002.png?featherlight=false&width=90pc)
4. In the **Stack name** section, type ```DevAx-05```.
* In the **EEKeyPair** section, seclect **KPforDevAxInstances**.
* Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-003.png?featherlight=false&width=90pc)
5. In the **Configure stack options** page, Drag the screen down, then Click **Next**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-004.png?featherlight=false&width=90pc)
6. In the **Review** page.
* Drag the screen down, then Click **I acknowledge that AWS CloudFormation might create IAM resources with custom names.**
* Click **Create stack**.
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-005.png?featherlight=false&width=90pc)
{{% notice note %}} 
**Cloudformation** will take 5 minutes to create stack. Wait until all stacks are shown in a **CREATE_COMPLETE** state.
{{% /notice %}}
![Create CloudFormation stack](/images/2-prepare/2.2-createstack/createstack-006.png?featherlight=false&width=90pc)

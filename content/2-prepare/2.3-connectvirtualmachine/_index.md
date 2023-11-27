+++
title = "Connect to the Windows Instance"
date = 2020
weight = 3
chapter = false
pre = "<b>2.3. </b>"
+++
#### Connect to the Windows Instance
1. Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* On the left navigation bar, click **Intances**.
* Select **DevAxWindowsHost**.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-001.png?featherlight=false&width=90pc)
2. In the **Connect to instance** page
* Click tab **RDP client**.
* Click **Download remote desktop file**. We will download file remote desktop to the folder contains the key pair.
* Click **Get password**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-002.png?featherlight=false&width=90pc)
3. In the **Get Windows password** page:
* Click **Browse**. 
* Select file **KPforDevAxInstances.pem** we downloaded in the section 1.1.
* Click **Decrypt Password** to decrypt the password. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-003.png?featherlight=false&width=90pc)
4. Copy decrypted password .
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-004.png?featherlight=false&width=90pc)
5. Open file **DevAxWindowsHost.rdp** we downloaded in the step 2.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-005.png?featherlight=false&width=30pc)
6. Type the password we copied in the step 4
* Click **OK**. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-006.png?featherlight=false&width=30pc)
7. Click **Don’t ask me again for connections to this computer**.
* Click **Yes**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-007.png?featherlight=false&width=30pc)
8. Connect successfully. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-008.png?featherlight=false&width=90pc)

#### Cấu hình AWS CLI
1. Assign the Administrator Access to user **awsstudent** was created by Cloud Formation template
* Go to [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Click user **awsstudent**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-009.png?featherlight=false&width=90pc)
2. In the **Permissions policies** section
* Click **Add permissions**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-010.png?featherlight=false&width=90pc)
3. In the **Add permissions to awsstudent** page
* Click **Attach existing policies directly**
* Type ```AdministratorAccess``` to the search bar.
* Select **AdministratorAccess**
* Click **Next:Review**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-011.png?featherlight=false&width=90pc)
4. Click **Add Permission**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-012.png?featherlight=false&width=90pc)
5. Click tab **Security credentials**
* Click **Create access key** to create the access key
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-013.png?featherlight=false&width=90pc)
6. Save **Access key** and **Secret access key** to use in the next steps
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-014.png?featherlight=false&width=90pc)
7. Execute the below command:
```
aws configure set profile.aws-lab-env.region <YOUR_REGION>
aws configure set profile.aws-lab-env.aws_access_key_id <YOUR_ACCESS_KEY_ID>
aws configure set profile.aws-lab-env.aws_secret_access_key <YOUR_SECRET_ACCESS_KEY>
git config --global user.email <YOUR_EMAIL>
git config --global user.name awsstudent
```
{{% notice note %}} 
Change **<your_region>** by **Region code** \
Change **<access_key_id>** by **Access Key Id** we saved in the step 6\
Change **<secret_access_key>** by **Secret Access Key** we saved in the step 6
{{% /notice %}}
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-015.png?featherlight=false&width=60pc)
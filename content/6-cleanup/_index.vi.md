+++
title = "Dọn dẹp tài nguyên"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++
Bạn sẽ dọn dẹp tài nguyên theo thứ tự sau:

#### Terminate EC2 Instance
1. Truy cập [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Trên thanh điều hướng bên trái, click **Intances**.
* Chọn các instance được tạo ra trong bài lab này. 
* Click **Instance state**
* Click **Terminate instance**
![Clean up reources](/images/6-cleanup/cleanup-001.png?featherlight=false&width=90pc)
2. Click **Terminate**
![Clean up reources](/images/6-cleanup/cleanup-002.png?featherlight=false&width=90pc)

#### Xóa Users
1. Truy cập vào [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Nhập ```awsstudent``` vào ô tìm kiếm
* Chọn **awsstudent**. 
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-003.png?featherlight=false&width=90pc)
2. Điền ```awsstudent``` để xác nhận, sau đó click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-004.png?featherlight=false&width=90pc)

#### Xóa CloudFormation Stack
1. Truy cập [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Chọn **DevAx-05**.
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-005.png?featherlight=false&width=90pc)
2. Click **Delete stack**
![Clean up reources](/images/6-cleanup/cleanup-006.png?featherlight=false&width=90pc)

#### Xóa Lambda function
1. Truy cập [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home).
* Click **Functions**.
* Chọn **Kinesissam-LambdaFunction**.
* Click **Delete**
![Clean up reources](/images/6-cleanup/cleanup-007.png?featherlight=false&width=90pc)
2. Điền ```delete``` để xác nhận, sau đó click **Delete** để xóa
![Clean up reources](/images/6-cleanup/cleanup-008.png?featherlight=false&width=90pc)

#### Xóa CodeCommit Repositories
1. Truy cập [**AWS CodeCommit console**](https://console.aws.amazon.com/codesuite/codecommit/home).
* Click **Repositories**
* Chọn **KinesisProducer**
* Click **Delete repository**
![Clean up reources](/images/6-cleanup/cleanup-009.png?featherlight=false&width=90pc)
2. Điền ```delete``` để xác nhận, sau đó click **Delete** để xóa
![Clean up reources](/images/6-cleanup/cleanup-010.png?featherlight=false&width=90pc)
3. Làm tương tự với CodeCommit Repository còn lại

#### Xóa S3 bucket
1. Truy cập vào [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/).
* Click **Buckets**
* Chọn S3 bucket đầu tiên.
* Click **Empty**.
![Clean up reources](/images/6-cleanup/cleanup-011.png?featherlight=false&width=90pc)
2. Điền ```permanently delete``` để xác nhận, sau đó click **Empty** để xóa toàn bộ dữ liệu trong S3 bucket.
![Clean up reources](/images/6-cleanup/cleanup-012.png?featherlight=false&width=90pc)
3. Click **Exit** để trở lại giao diện S3.
![Clean up reources](/images/6-cleanup/cleanup-013.png?featherlight=false&width=90pc)
4. Click **Delete**.
![Clean up reources](/images/6-cleanup/cleanup-014.png?featherlight=false&width=90pc)
5. Điền tên bucket sau đó click **Delete bucket** để xóa S3 bucket.
![Clean up reources](/images/6-cleanup/cleanup-015.png?featherlight=false&width=90pc)
6. Làm tương tự cho S3 bucket còn lại
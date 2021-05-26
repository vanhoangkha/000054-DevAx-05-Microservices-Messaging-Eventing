+++
title = "Dùng Eclipse"
date = 2021
weight = 1
chapter = false
pre = "<b>Cách A. </b>"
+++

Trong thiết lập bài thực hành đã cung cấp một EC2 instance để bạn chạy với tư cách Producer trong tình huống này. Bây giờ bạn sẽ kết nối với EC2 instance, sao chép kho lưu trữ về, build và chạy chương trình Producer. Lưu ý rằng quy trình này giống với quy trình bạn vừa hoàn thành trên Consumer EC2 instance, nhưng chúng tôi lặp lại các hướng dẫn ở đây để bạn tiện theo dõi.

**Nội dung**
- [Thiết lập Kinesis Producer](#thiết-lập-kinesis-producer)
- [Cấu hình mapping index cho ElasticSearch](#cấu-hình-mapping-index-cho-elasticsearch)

#### Thiết lập Kinesis Producer

1. Trong **AWS Console**, trang **CloudFormation**, trong tab **Outputs**, tìm giá trị **EC2InstanceKinesisProducer** và sao chép vào bộ nhớ tạm.

![ElasticSearch](/images/3/11.png?width=90pc)

2. Sử dụng lệnh mẫu sau để kết nối đến EC2 instance của bạn:
```bash
ssh -i <path to your downloaded PEM file> ec2-user@<EC2InstanceKinesisProducer>
```

**Ví dụ:**
```bash
ssh -i ~/Downloads/ee-default-keypair.pem ec2-user@34.212.21.7
```

3. Ở cửa sổ lệnh bạn SSH vào instance, sử dụng lệnh sau để tải về từ kho dữ liệu của bạn:
 
```bash
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
git clone https://git-codecommit.ap-northeast-1.amazonaws.com/v1/repos/KinesisProducer
```

4. Mã nguồn bạn đã tải lên từ máy trạm của mình bây giờ sẽ được sao chép vào EC2 instance và sẽ mất một chút thời gian để hoàn thành.
5. Build mã nguồn ứng dụng. `cd` vào thư mục `KinesisProducer` và thực thi lệnh:
```bash
cd KinesisProducer
/opt/apache-maven-3.3.9/bin/mvn package shade:shade
```

Sẽ mất một chút thời gian để build mã nguồn ứng dụng.

6. Producer cần dữ liệu nguồn và sẽ sử dụng một tập tin CSV trong thư mục làm việc hiện tại, được gọi là `SalesData.csv`. Sao chép tập tin vào EC2 instance bằng cách sử dụng lệnh ```wget``` hoặc ```curl``` đến URL tập tin. Bạn có thể lấy URL của tập tin bằng cách nhấp chuột phải vào đường dẫn sau và sao chép địa chỉ liên kết của nó: [**SalesData.csv**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/SalesData.csv).

#### Cấu hình mapping index cho ElasticSearch

Khi dữ liệu được gửi đến ElasticSearch, nó sẽ cố gắng suy ra các kiểu dữ liệu trong nội dung và thường thực hiện tốt việc xác định đúng các kiểu dữ liệu. Trong trường hợp của chúng ta, chúng ta đang gửi ngày/giờ của mỗi bản ghi bán hàng dưới kiểu **long** và vì vậy ElasticSearch sẽ diễn giải trường này chỉ là một số (kiểu long). Thay vào đó, chúng ta cần cảnh báo ElasticSearch rằng nó sẽ diễn giải trường dấu thời gian này là ***Date**. Chúng ta thực hiện việc này bằng cách tạo các **Mapping** trong chỉ mục trước khi chúng ta gửi bất kỳ bản ghi nào.

7. Trên cửa sổ dòng lệnh, bạn có kết nối SSH tới **Kinesis Consumer** EC2 instance, hãy sử dụng lệnh sau - nhưng thay thế bằng giá trị ***ElasticsearchEndpoint*** từ bảng kết quả ở tab **Cloudformation Outputs**:
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

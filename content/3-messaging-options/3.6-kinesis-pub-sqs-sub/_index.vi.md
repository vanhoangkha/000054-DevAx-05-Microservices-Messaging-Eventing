+++
title = "Kinesis Publisher tới SQS Subcriber "
date = 2020
weight = 6
chapter = false
pre = "<b>3.6. </b>"
+++
#### Kinesis Publisher tới SQS Subcriber 

Amazon Kinesis giúp bạn dễ dàng thu thập, xử lý và phân tích luồng dữ liệu truyền liên tục, real-time để bạn có thể nhận được thông tin chi tiết kịp thời và phản ứng nhanh với thông tin mới. Trong trường hợp này, trình duyệt sẽ gửi các điểm vẽ tới Kinesis stream. Thông thường, chúng ta sẽ tạo một Kinesis consumer client và đọc trực tiếp các sự kiện từ stream và xử lý chúng, và chúng ta sẽ thực hiện điều này trong kịch bản tiếp theo. Đối với tình huống này, giả sử chúng ta muốn các thông tin được gửi đến hàng đợi SQS giống như chúng ta đã làm trong các tình huống trước đây.

Vì không có cách nào được xây dựng sẵn để định tuyến các message đã được gửi tới Kinesis stream vào hàng đợi SQS, chúng ta cần một bên trung gian thực hiện việc xử lý/ định tuyến đó cho chúng ta. Trong trường hợp này, chúng ta sẽ triển khai trung gian dưới dạng một hàm AWS Lambda sẽ tạo cầu nối giữa Kinesis stream và hàng đợi SQS mà chúng ta xác định làm mục tiêu.

Kiến trúc cho kịch bản này trông giống như sau:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesislambdasqsarchitecture.png?featherlight=false&width=70pc)
Tương tự như kịch bản trước, message được hiển thị bởi một subscriber hàng đợi Amazon SQS. Nhưng trong trường hợp này, các message được gửi đến một Kinesis stream, và sau đó được chuyển đến hàng đợi SQS bởi hàm Lambda.

Để bắt đầu bản demo này, hãy chọn vào danh sách trên trang web và chọn **AWS Kinesis publisher to SQS subscriber**. Khi trang khởi chạy, hãy vẽ một hình dạng trên khung của Kinesis Publisher. Lưu ý rằng bộ đếm **Messages Sent** tăng lên, nhưng không có hình dạng nào được hiển thị trên khung subscriber SQS? Điều này là do các message đang được gửi vào Kinesis stream, nhưng không có Kinesis consumers nào đang xử lý message:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-001.png?featherlight=false&width=90pc)

#### Tạo một hàm AWS Lambda triển khai Amazon Kinesis event consumer

1. Mở **Eclipse IDE**.
* Click dấu mũi tên xuống cạnh biểu tượng AWS
* Click **New AWS Lambda java Project…**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-002.png?featherlight=false&width=90pc)
2. Trong phần **Create a new AWS Lambda Java project**
* Tại mục **Project name**, nhập ```KinesisToSQS```
* Tại mục **Input Type**, chon **Kinesis Event**
* Click **Finish**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-003.png?featherlight=false&width=90pc)
3. Click chuột phải vào file có đường dẫn **KinesisToSQS\src\main\java\com\amazonaws\lambda\demo\LambdaFunctionHandler.java**
* Click **Amazon Web Services**
* Click **Upload function to AWS Lambda…**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-004.png?featherlight=false&width=90pc)
4. Trong phần **Select Target AWS Lambda Function**
* Tại mục **Select the AWS Region**, chọn **Region** của bạn
* Tại mục **Create a new Lambda function**, nhập ```Kinesissam-LambdaFunction```
* Click **Next**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-005.png?featherlight=false&width=90pc)
5. Trong phần **Function Configuration**
* Tại mục **Description**, nhập ```Lab 5 Kinesis to SQS```
* Tại mục **IAM Role**, chọn **LambdaKinesisToSQSRole**
* Click **Finish**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-006.png?featherlight=false&width=90pc)
6. Trong khi bạn đang đợi tải lên hoàn tất, hãy xem nhanh cách triển khai đơn giản mà IDE đã tạo cho bạn. Mở class **LambdaFunctionHandler**. Chúng sẽ tương tự như sau:
```
public class LambdaFunctionHandler implements RequestHandler<KinesisEvent, Integer> {

    @Override
    public Integer handleRequest(KinesisEvent event, Context context) {
        context.getLogger().log("Input: " + event);

        for (KinesisEventRecord record : event.getRecords()) {
            String payload = new String(record.getKinesis().getData().array());
            context.getLogger().log("Payload: " + payload);
        }

        return event.getRecords().size();
    }
}
```
7. Truy cập [**AWS Kinesis console**](https://console.aws.amazon.com/kinesis/home)
* Click **Data streams**
* Bạn sẽ thấy hai stream - cả hai đều đã được cung cấp bởi template của bài thực hành trong quá trình thiết lập. Click **iDevelopDrawingData**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-007.png?featherlight=false&width=90pc)
8. Chọn tab **Monitoring** và cuộn xuống để hiển thị biểu đồ **Incoming data - sum (Count)**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-008.png?featherlight=false&width=90pc)
Xung quanh vị trí này sẽ là những thống kê khác về stream. Lưu ý rằng **Incoming data - sum (Count)** khớp với cùng một số bạn thấy trong trang web demo của chúng ta, trong **Messages Sent**. Bạn có thể cần phải di chuyển chuột qua biểu đồ để xem khối lượng dữ liệu thực tế. Trong ví dụ được hiển thị ở đây, bạn sẽ trỏ chuột vào cuối dòng để hiển thị số lượng (hiển thị **206**). Số điểm của hai điểm trên biểu đồ sẽ bằng số liệu **Messages Sent** trên trang web của chúng ta (hình ảnh cuối cùng, hiển thị **Messages Sent** từ trang web của chúng ta. **206**)
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-009.png?featherlight=false&width=90pc)
9. Truy cập [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home)
* Click **Functions**
* Nhập ```Kinesissam-LambdaFunction``` vào ô tìm kiếm, nhấn **Enter**
* Click **Kinesissam-LambdaFunction**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-010.png?featherlight=false&width=90pc)
10. Click **Add trigger**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-011.png?featherlight=false&width=90pc)
11. Trong trang **Add trigger**
* Chọn **Kinesis**
* Tại mục **Kinesis stream**, chọn **iDevelopDrawingData**
* Tại mục **Starting position**, chọn **Trim horizon**
* Click **Add**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-012.png?featherlight=false&width=90pc)
12. Sẽ mất khoảng thời gian để hàm Lambda được gắn vào Kinesis stream. Chờ một phút, click **Refresh Icon**
* Click **Details**, bạn sẽ thấy thông báo cho biết **Last processing result: OK**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-013.png?featherlight=false&width=90pc)
13. Chọn tab **monitor**. Các điểm vẽ đã được gửi vào Kinesis stream sẽ được sử dụng bởi chức năng Lambda và bạn sẽ thấy các CloudWatch metric cho biết hoạt động này. Bạn sẽ thấy hoạt động trong biểu đồ Invocation count và Invocation duration graphs. Kiểm tra để đảm bảo rằng bạn không thấy bất kỳ Invocation error nào.
{{% notice note %}} 
Tùy thuộc vào số lượng điểm vẽ bạn đã xuất vào Kinesis stream, bạn sẽ thấy một số khác nhau cho Invocation count. Khi chúng ta bật trigger, chúng ta đặt Batch size thành 100 (là giá trị mặc định). Vì vậy, hàm Lambda sẽ được gọi với tối đa 100 bản ghi cùng một lúc. Nếu bạn đã gửi 225 message vào luồng, việc này sẽ sử dụng 3 invocation của hàm Lambda - Math.ceil(225/100) == 3.
{{% /notice %}}
* Click **View logs in CloudWatch** để mở CloudWatch Log Stream cho hàm Lambda này.
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-014.png?featherlight=false&width=90pc)
14. Click **Log stream** đầu tiên trong danh sách
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-015.png?featherlight=false&width=90pc)
15. Cuộn qua các sự kiện trong kết quả ở CloudWatch Log. Các sự kiện này đã được ghi vào nhật ký bằng hàm Lambda của chúng ta:
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-016.png?featherlight=false&width=90pc)

Khi viết mã hàm Lambda của bạn bằng Java, bạn có thể phát chuỗi nhật ký vào CloudWatch log bằng cách chỉ cần gọi phương thức .log() của Logger:
```
context.getLogger().log("Log message to emit: " + someMessage);
```
#### Cập nhật hàm Lambda đã triển khai để gửi các sự kiện đã nhận vào hàng đợi SQS

Quay lại tình huống của chúng ta, chúng ta muốn đọc các message từ Kinesis stream và gửi chúng vào hàng đợi SQS, nơi triển khai trên trình duyệt của chúng ta để có thể đọc chúng và vẽ chúng trên khung của subscriber.

1. Trong Eclipse IDE, thay toàn bộ nội dung của tập tin **KinesisToSQS/src/main/java/com/amazonaws/lambda/demo/LambdaFunctionHandler.java** bằng đoạn mã sau:
```
package com.amazonaws.lambda.demo;

import java.awt.geom.AffineTransform;
import java.awt.geom.Point2D;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import com.amazonaws.services.lambda.runtime.events.KinesisEvent;
import com.amazonaws.services.lambda.runtime.events.KinesisEvent.KinesisEventRecord;
import com.amazonaws.services.sqs.AmazonSQS;
import com.amazonaws.services.sqs.AmazonSQSClientBuilder;
import com.amazonaws.services.sqs.model.SendMessageBatchRequest;
import com.amazonaws.services.sqs.model.SendMessageBatchRequestEntry;
import com.fasterxml.jackson.databind.ObjectMapper;

public class LambdaFunctionHandler implements RequestHandler<KinesisEvent, Integer> {

	final int MAX_SQS_BATCH_SIZE = 10;

	private AmazonSQS sqs = AmazonSQSClientBuilder.defaultClient();

    @Override
    public Integer handleRequest(KinesisEvent event, Context context) {

    		Iterator<KinesisEventRecord> iterator = event.getRecords().iterator();    		
    		AffineTransform transformer = new AffineTransform(1, 0, 0, -1, 0, 400);
    		ObjectMapper mapper = new ObjectMapper();
    		Point2D dstPoint = new Point2D.Double();
    		boolean sendMessagesToSQS = true;
    		
    		try
    		{
    			//
    			// Read the SQS Queue Target from the Lambda Environment Variables
    			//
    			String sqsUrl = System.getenv("TargetSQSUrl");
    			if ( sqsUrl == null || sqsUrl.isEmpty() )
    			{
    				context.getLogger().log("WARNING:: Environment Variable [TargetSQSUrl] is not set. No messages will be sent via SQS");
    				sendMessagesToSQS = false;
    			}
    				
	    		while ( iterator.hasNext() )
	    		{
	    			int messageCounter = 0;
	
	        		// Prepare a batch request to write all the messages we have received in this invocation
	        		Collection<SendMessageBatchRequestEntry> entries = new ArrayList<SendMessageBatchRequestEntry>();
	
	        		while ( iterator.hasNext() && messageCounter++ < MAX_SQS_BATCH_SIZE )
	        		{
		            String payload = new String(iterator.next().getKinesis().getData().array());
		            context.getLogger().log("Payload: " + payload);
		            DrawPoint transformedPoint = new DrawPoint();
	
		            try {
	
						transformedPoint = mapper.readValue(payload, DrawPoint.class);
			            // Transform the point
			            Point2D srcPoint = new Point2D.Double(transformedPoint.getX(), transformedPoint.getY());	            
			            transformer.transform(srcPoint, dstPoint);
	
			            // Update the payload
			            transformedPoint.setX((int)dstPoint.getX());
			            transformedPoint.setY((int)dstPoint.getY());
	
		                 // Add this payload into our batch
		        		    	SendMessageBatchRequestEntry entry = new SendMessageBatchRequestEntry(
		        		    			"msg_" + messageCounter,
		        		    			mapper.writeValueAsString(transformedPoint)
		        		    			);
	
		        		    	entries.add(entry);
					}
		            catch (Exception e)
		            {
		            		context.getLogger().log("Unable to deserialise " + payload + " as a DrawPoint! " + e.getMessage());					
					}
	        		}
	
	        		if ( sendMessagesToSQS && entries.size() > 0 )
	        		{
	        			// We have reached the end of the records or we have reached the maximum	        		
		        		// batch size allowed for SQS, so we need to send our entries
		        		context.getLogger().log("Sending batch of " + (messageCounter - 1) + " events to SQS...");
		
		        		SendMessageBatchRequest batch = new SendMessageBatchRequest()
		        				.withQueueUrl(sqsUrl);
		
		            batch.setEntries(entries);
		
		            // Perform the message sending
		            sqs.sendMessageBatch(batch); 
	        		}
	    		}
    		}
		catch (Exception e)
        {
        		context.getLogger().log("EXCEPTION::Aborting Lambda processing");
        		context.getLogger().log(e.getStackTrace().toString());
		}
    		
        return event.getRecords().size();
    }

    // Inner class must be marked as static in order for the JSON mapper to deserialise
	private static class DrawPoint {

		private int x;
		private int y;
		private long timestamp;
		private boolean clear;
		
		public int getX() {
			return x;
		}
		public void setX(int x) {
			this.x = x;
		}
		public int getY() {
			return y;
		}
		public void setY(int y) {
			this.y = y;
		}
		public long getTimestamp() {
			return timestamp;
		}
		public void setTimestamp(long timestamp) {
			this.timestamp = timestamp;
		}
		public boolean isClear() {
			return clear;
		}
		public void setClear(boolean clear) {
			this.clear = clear;
		}	
	}
}
```
* Lưu lại
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-017.png?featherlight=false&width=90pc)
2. Mở **Command Prompt** và chuyển đường dẫn đến project **KinesisToSQS**
* Chạy câu lệnh ```mvn package``` để build
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-018.png?featherlight=false&width=60pc)
3. Truy cập [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home)
* Click **Functions**
* Nhập ```Kinesissam-LambdaFunction``` vào ô tìm kiếm, nhấn **Enter**
* Click **Kinesissam-LambdaFunction**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-019.png?featherlight=false&width=90pc)
4. Để cung cấp function package, trong phần **Code soure**
* Click Upload from
* Click **.zip or .jar file**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-020.png?featherlight=false&width=90pc)
5. Click **Upload**, Chọn file **demo-1.0.0.jar** trong thư mục **target** của projetc **KinesisToSQS** đã build trong bước 2
* Click **Save**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-021.png?featherlight=false&width=90pc)
6. Click tab **Configuration**
* Click **Environment variables**
* Click **Edit**
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-022.png?featherlight=false&width=90pc)
7. Trong trang **Edit Environment variables**
* Click **Add environment variable**
* Tại mục **Key**, nhập **TargetSQSUrl**
* Tại mục **Value**, nhập giá trị của **QueueUrl1** đã lưu tại bước 3 phần 3.1
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-023.png?featherlight=false&width=90pc)
8. Quay lại trang web demo của chúng ta và vẽ một hình dạng mới để tạo ra các sự kiện vào Kinesis stream.
![Kinesis Publisher To SQS Subscriber](/images/3-messaging-options/3.6-kinesis-pub-sqs-sub/kinesis-pub-sqs-sub-024.png?featherlight=false&width=90pc)

Các điểm vẽ bây giờ sẽ được nhận qua hàng đợi SQS và được hiển thị trên khung. Lưu ý rằng hình dạng bạn vẽ có bị đảo ngược theo chiều Y không? Điều này là do việc triển khai chúng ta đã sử dụng cho hàm Lambda thực hiện một phép biến đổi trên các điểm vẽ, việc này chứng minh chúng ta có thể thao tác dữ liệu trong Kinesis stream trước khi chuyển nó vào hàng đợi SQS.

Hãy dành một chút thời gian để xem lại mã triển khai handler để đảm bảo bạn hiểu cách hoạt động của nó.

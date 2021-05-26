+++
title = "Dùng Eclipse"
date = 2021
weight = 1
chapter = false
pre = "<b>Cách A. </b>"
+++

Amazon Kinesis giúp bạn dễ dàng thu thập, xử lý và phân tích luồng dữ liệu truyền liên tục, real-time để bạn có thể nhận được thông tin chi tiết kịp thời và phản ứng nhanh với thông tin mới. Trong trường hợp này, trình duyệt sẽ gửi các điểm vẽ tới Kinesis stream. Thông thường, chúng ta sẽ tạo một Kinesis consumer client và đọc trực tiếp các sự kiện từ stream và xử lý chúng, và chúng ta sẽ thực hiện điều này trong kịch bản tiếp theo. Đối với tình huống này, giả sử chúng ta muốn các thông tin được gửi đến hàng đợi SQS giống như chúng ta đã làm trong các tình huống trước đây.

Vì không có cách nào được xây dựng sẵn để định tuyến các message đã được gửi tới Kinesis stream vào hàng đợi SQS, chúng ta cần một bên trung gian thực hiện việc xử lý/ định tuyến đó cho chúng ta. Trong trường hợp này, chúng ta sẽ triển khai trung gian dưới dạng một hàm AWS Lambda sẽ tạo cầu nối giữa Kinesis stream và hàng đợi SQS mà chúng ta xác định làm mục tiêu.

Kiến trúc cho kịch bản này trông giống như sau:

![SQS](/images/2/kinesislambdasqsarchitecture.png?width=50pc)

Tương tự như kịch bản trước, message được hiển thị bởi một subscriber hàng đợi Amazon SQS. Nhưng trong trường hợp này, các message được gửi đến một Kinesis stream, và sau đó được chuyển đến hàng đợi SQS bởi hàm Lambda.

**Nội dung**
- [Tạo một hàm AWS Lambda triển khai Amazon Kinesis event consumer](#tạo-một-hàm-aws-lambda-triển-khai-amazon-kinesis-event-consumer)
- [Cập nhật hàm Lambda đã triển khai để gửi các sự kiện đã nhận vào hàng đợi SQS](#cập-nhật-hàm-lambda-đã-triển-khai-để-gửi-các-sự-kiện-đã-nhận-vào-hàng-đợi-sqs)


1. Để bắt đầu bản demo này, hãy chọn vào danh sách trên trang web và chọn **AWS Kinesis publisher to SQS subscriber**. Khi trang khởi chạy, hãy vẽ một hình dạng trên khung của Kinesis Publisher. Lưu ý rằng bộ đếm **Messages Sent** tăng lên, nhưng không có hình dạng nào được hiển thị trên khung subscriber SQS? Điều này là do các message đang được gửi vào Kinesis stream, nhưng không có Kinesis consumers nào đang xử lý message: 

![SQS](/images/2/15.png?width=90pc)

Làm theo các bước bên dưới để triển khai hàm Lambda.

#### Tạo một hàm AWS Lambda triển khai Amazon Kinesis event consumer

2. Trong **Eclipse IDE**, chọn icon **AWS** để xem AWS menu, và chọn **New AWS Lambda java Project...**

![SQS](/images/2/16.png?width=90pc)

3. Trong cửa sổ hiển thị, ở **Project name** nhập ```KinesisToSQS```
4. Ở **Input Type**, chọn `Kinesis Event` từ danh sách.

![SQS](/images/2/17.png?width=90pc)

5. Chọn **Finish**
 
Chúng ta sẽ sử dụng cách triển khai mặc định để kiểm tra xem cấu hình có đúng không trước khi cập nhật cách triển khai nhằm thực hiện một tác vụ cụ thể.

6. Chuột phải vào tập tin **LambdaFunctionHandler.java** và chọn menu **Amazon Web Services**. Từ menu con xuất hiện, hãy chọn **Upload function to AWS Lambda...**

![SQS](/images/2/18.png?width=90pc)

7. Chắc chắn rằng Region **Asia Pacific(Tokyo)** đã được chọn, và ở **Create a new Lambda function**: nhập vào ```kinesissam-202103-LambdaFunction-XXXXXXXXXXXX```.
8. Chọn **Next**.

![SQS](/images/2/19.png?width=90pc)

9. Nhập vào **Description** là ```Lab 5 Kinesis to SQS```.
10. Ở **IAM Role** chọn ```LambdaKinesisToSQSRole``` đã được bài thực hành cung cấp cho bạn.
11. Chọn **Finish**. Hàm Lambda sẽ được tải lên tài khoản AWS của bạn, sẵn sàng để kiểm thử. Quá trình này có thể mất một hoặc hai phút.

Trong khi bạn đang đợi tải lên hoàn tất, hãy xem nhanh cách triển khai đơn giản mà IDE đã tạo cho bạn.

12. Mở class **LambdaFunctionHandler**. Chúng sẽ tương tự như sau:

```java
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

Việc triển khai này sẽ đơn giản là ghi nhật ký sự kiện đầu vào mà hàm Lambda nhận được, và sau đó đối với mỗi bản ghi trong gói, chúng sẽ ghi lại nội dung. Đây là một điểm khởi đầu tốt để cho phép chúng ta xác nhận rằng cấu hình của chúng ta đang đọc chính xác dữ liệu từ Kinesis stream.

13. Mở **Kinesis console** trong trình duyệt và chọn **Go to the Streams console**

![SQS](/images/2/20.png?width=90pc)

14. Bạn sẽ thấy hai stream - cả hai đều đã được cung cấp bởi template của bài thực hành trong quá trình thiết lập. Bấm vào **iDevelopDrawingData**.

![SQS](/images/2/21.png?width=90pc)

15. Chọn tab **Monitoring** và cuộn xuống để hiển thị biểu đồ **Incoming data - sum (Count)**.

![SQS](/images/2/22.png?width=90pc)

Xung quanh vị trí này sẽ là những thống kê khác về stream. Lưu ý rằng **Incoming data - sum (Count)** khớp với cùng một số bạn thấy trong trang web demo của chúng ta, trong **Messages Sent**? Bạn có thể cần phải di chuyển chuột qua biểu đồ để xem khối lượng dữ liệu thực tế. Trong ví dụ được hiển thị ở đây, bạn sẽ trỏ chuột vào cuối dòng để hiển thị số lượng (hiển thị 118). Số điểm của hai điểm trên biểu đồ sẽ bằng số liệu **Messages Sent** trên trang web của chúng ta (hình ảnh cuối cùng, hiển thị **Messages Sent** từ trang web của chúng ta. 118)

![SQS](/images/2/23.png?width=90pc)

![SQS](/images/2/25.png?width=90pc)

16. Khi hoàn tất quá trình tải lên hàm Lambda, hãy mở **AWS Lambda console** trong trình duyệt. Bạn sẽ thấy hàm Lambda mới mà bạn vừa tải lên, ```kinesissam-202103-LambdaFunction-XXXXXXXXXXXX```. Chọn vào nó để quản lý cấu hình của nó.
17. Trong Designer dashboard, bạn sẽ tìm thấy phần **Add triggers**.

![SQS](/images/2/26.png?width=90pc)

18. Chọn **Kinesis** từ danh sách các triggers. Nó sẽ mở lên giao diện **Trigger configuration**.
19. Ở **Kinesis stream** chọn ```iDevelopDrawingData``` từ danh sách.
20. Ở **Starting position** chọn ```Trim horizon```. Giữ thiết lập **Batch size** ở ```100```.

![SQS](/images/2/27.png?width=90pc)

21. Chọn **Add** sau đó chọn **Save**. Kinesis trigger sẽ được kích hoạt cho hàm Lambda.

![SQS](/images/2/28.png?width=90pc)

22. Sẽ mất khoảng thời gian để hàm Lambda được gắn vào Kinesis stream. Chờ một phút, sau đó chọn vào biểu tượng **Refresh** trên pannel **Kinesis**. Khi hàm Lambda được kết nối, bạn sẽ thấy thông báo cho biết *Last processing result: OK*. 

![SQS](/images/2/29.png?width=90pc)

23. Chọn vào tab **Monitoring**. Các điểm vẽ đã được gửi vào Kinesis stream sẽ được sử dụng bởi chức năng Lambda và bạn sẽ thấy các CloudWatch metric cho biết hoạt động này. Bạn sẽ thấy hoạt động trong biểu đồ *Invocation count* và *Invocation duration graphs*. Kiểm tra để đảm bảo rằng bạn không thấy bất kỳ *Invocation error* nào. 

{{% notice note %}}
Tùy thuộc vào số lượng điểm vẽ bạn đã xuất vào Kinesis stream, bạn sẽ thấy một số khác nhau cho *Invocation count*. Khi chúng ta bật trigger, chúng ta đặt Batch size thành 100 (là giá trị mặc định). Vì vậy, hàm Lambda sẽ được gọi với tối đa 100 bản ghi cùng một lúc. Nếu bạn đã gửi 225 message vào luồng, việc này sẽ sử dụng 3 invocation của hàm Lambda - Math.ceil(225/100) == 3.
{{% /notice %}}

![SQS](/images/2/30.png?width=90pc)

24. Chọn vào **View logs in CloudWatch** để mở CloudWatch Log Stream cho hàm Lambda này. Chọn vào luồng nhật ký đầu tiên trong danh sách và xem lại các nhật ký được liệt kê:

![SQS](/images/2/31.png?width=90pc)

25. Cuộn qua các sự kiện trong kết quả ở CloudWatch Log. Các sự kiện này đã được ghi vào nhật ký bằng hàm Lambda của chúng ta:

![SQS](/images/2/32.png?width=90pc)

Khi viết mã hàm Lambda của bạn bằng Java, bạn có thể phát chuỗi nhật ký vào CloudWatch log bằng cách chỉ cần gọi phương thức .log() của Logger:

```bash
context.getLogger().log("Log message to emit: " + someMessage);
```

#### Cập nhật hàm Lambda đã triển khai để gửi các sự kiện đã nhận vào hàng đợi SQS

Quay lại tình huống của chúng ta, chúng ta muốn đọc các message từ Kinesis stream và gửi chúng vào hàng đợi SQS, nơi triển khai trên trình duyệt của chúng ta để có thể đọc chúng và vẽ chúng trên khung của subscriber.

26. Trong **Eclipse IDE**, thay **toàn bộ nội dung** của tập tin ```/KinesisToSQS/src/main/java/com/amazonaws/lambda/demo/LambdaFunctionHandler.java``` bằng đoạn mã sau:

```java
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

Bạn cũng có thể tải xuống tập tin này từ [**đây**](https://workshops.devax.academy/monoliths-to-microservices/module5/files/LambdaFunctionHandler.java).

27. Tải lại hàm Lambda bằng cách sử dụng Eclipse IDE.
28. Việc triển khai hàm Lambda mong đợi **SQS Queue Url** được chuyển vào thông qua **Environment Variable**. Trước khi hàm có thể gửi message đến hàng đợi SQS, bạn cần phải cấu hình biến này.

Sau khi quá trình tải lên hoàn tất, trên **AWS Lambda console**, chọn vào tab **Configuration** cho hàm Lambda `kinesissam-202103-LambdaFunction-XXXXXXXXXXXX`. Cuộn xuống phần **Environment variables** (nếu bạn không thấy, hãy chọn vào tên hàm Lambda của bạn trong Designer dashboard) và thêm một biến mới bằng khóa ```TargetSQSUrl``` và đặt giá trị thành ```QueueUrl1``` từ phòng thí nghiệm **Additional Info panel**. \
Chọn vào **Save**.

![SQS](/images/2/33.png?width=90pc)

29. Bây giờ, hãy quay lại trang web demo của chúng ta và vẽ một hình dạng mới để tạo ra các sự kiện vào Kinesis stream.

![SQS](/images/2/34.png?width=90pc)

Các điểm vẽ bây giờ sẽ được nhận qua hàng đợi SQS và được hiển thị trên khung. Lưu ý rằng hình dạng bạn vẽ có bị đảo ngược theo chiều Y không? Điều này là do việc triển khai chúng ta đã sử dụng cho hàm Lambda thực hiện một phép biến đổi trên các điểm vẽ, việc này chứng minh chúng ta có thể thao tác dữ liệu trong Kinesis stream trước khi chuyển nó vào hàng đợi SQS.

Hãy dành một chút thời gian để xem lại mã triển khai handler để đảm bảo bạn hiểu cách hoạt động của nó.

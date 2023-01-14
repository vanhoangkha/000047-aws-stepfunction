+++
title = "Kiểm tra workflow"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3.3 </b>"
+++


#### Kiểm tra workflow


1. Truy cập vào [giao diện State machines](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click vào state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
  + Đây là phiên bản được triển khai lại của state machine của chúng ta. Phiên bản mới  không thay đổi, ngoại trừ việc chúng ta đã cấp IAM role của nó để gọi lambda function **Data Checking**. Hãy thử thực hiện lại nó với một số đầu vào mẫu để xem điều gì sẽ xảy ra.
 
![AWS Step Functions](/images/3.3.3/0001.png?featherlight=false&width=90pc)

2. Click **Start execution** để thực hiện lại workflow.
3. Điền nội dung JSON dưới đây vào mục input.
```
{
    "application": { 
        "name": "Spock", 
        "address": "123 Enterprise Street" 
    }
}
```
  + Click **Start execution**.

![AWS Step Functions](/images/3.3.3/0002.png?featherlight=false&width=90pc)

{{%notice tip%}}
Sau một lúc, bạn sẽ thấy rằng việc thực thi không thành công. Tuy nhiên, lần này, chúng ta không có bất kỳ trạng thái màu đỏ nào, bởi vì lần này chúng ta gặp phải lỗi khác. \
Trạng thái **Check Name** đã thực thi thành công, tuy nhiên trạng thái **Check Address** thì có màu xám. Nếu bạn nhìn vào mã màu ở cuối phần hình ảnh hóa, bạn sẽ thấy rằng điều này có nghĩa là trạng thái đã bị hủy. Hãy cùng xem tại sao.
{{%/notice%}}

![AWS Step Functions](/images/3.3.3/0003.png?featherlight=false&width=90pc)


1. Kéo màn hình xuống dưới tới phần **Execution event history**.
 + Click vào lần thực thi cuối cùng.
 + Chúng ta sẽ thấy lỗi như bên dưới
 ```
 {
  "error": "States.Runtime",
  "cause": "An error occurred while executing the state 'Check Address' (entered at the event id #7). The JSONPath '$.application.address' specified for the field 'address.$' could not be found in the input '{\"flagged\":false}'"
}
```

![AWS Step Functions](/images/3.3.3/0004.png?featherlight=false&width=90pc)


 Nếu bạn nhìn lại định nghĩa state machine của chúng ta cho trạng thái **Check Address** , bạn sẽ thấy rằng nó mong đợi có một đối tượng **application** trong đầu vào của nó và nó cố gắng chuyển **application.address** xuống Lambda function **Data Checking**.
 
![AWS Step Functions](/images/3.3.3/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0006.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0008.png?featherlight=false&width=90pc)

Thông báo lỗi cho chúng ta biết rằng state machine không thể tìm thấy application.address trong đầu vào của trạng thái. Để hiểu lý do tại sao, chúng ta cần tìm hiểu thêm một chút về cách một trạng thái hoạt động tạo ra đầu ra của nó và chuyển nó đến đầu vào của trạng thái kế tiếp trong bước tiếp theo.
 
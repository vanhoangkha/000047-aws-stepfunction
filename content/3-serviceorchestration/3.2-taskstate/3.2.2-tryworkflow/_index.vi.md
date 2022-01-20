+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2.2 </b>"
+++


#### Kiểm tra workflow
Cảnh báo mà chúng ta thấy ngay khi cập nhật định nghĩa state machine của mình là chính xác. Chúng ta sẽ cần cập nhật các quyền của IAM role của mình để state machine có thể hoạt động. Nhưng dù sao chúng ta hãy thử một lần thực thi khác để xem lỗi không đủ quyền sẽ như thế nào.

1. Click **Start execution**.

![StepFunctions](/images/SF/035.png?width=90pc)

2. Dán nội dung JSON bên dưới vào phần input. Sau đó click **Start execution**.
```
{
    "application": { 
        "name": "Spock", 
        "address": "123 Enterprise Street" 
    }
}

```

![StepFunctions](/images/SF/036.png?width=90pc)

{{%notice tip%}}
Sau một lúc, bạn sẽ thấy kết quả của việc thực thi không thành công này. "Execution status" hiển thị "Failed" và bạn sẽ thấy báo đỏ trong phần biểu diễn qui trình của state machine bên dưới, làm nổi bật trạng thái gặp lỗi.
{{%/notice%}}

![StepFunctions](/images/SF/037.png?width=90pc)

3. Click vào state **Check Name**, sau đó click tab **Exception**. Chúng ta có thể xem được chi tiết lỗi như hình dưới:


![StepFunctions](/images/SF/038.png?width=90pc)

Khi state machine  thực thi, nó đảm nhận IAM role để xác định loại hành động nào nó được phép thực hiện bên môi trường AWS. Hiện tại, chúng ta chưa thêm bất kỳ quyền rõ ràng nào để cho phép Role này gọi Lambda function **Data Checking** của chúng ta, vì vậy chúng ta gặp lỗi khi state machine này cố gắng chạy.

Chúng ta sẽ cùng khắc phục điều này bằng cách thêm các quyền thích hợp vào Role mà Step Functions của chúng ta đảm nhận trong quá trình thực thi ở bước kế tiếp.
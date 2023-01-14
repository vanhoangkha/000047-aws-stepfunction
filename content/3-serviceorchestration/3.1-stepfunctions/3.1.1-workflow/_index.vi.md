+++
title = "Khởi tạo workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1.1 </b>"
+++


#### Khởi tạo workflow

Để bắt đầu, chúng ta hãy thử lập mô hình các bước liên quan để kiểm tra tên, kiểm tra địa chỉ và sau đó phê duyệt đơn đăng ký. Quy trình làm việc đơn giản của chúng ta sẽ bắt đầu như sau:

![StepFunctions](/images/SF/020.png?width=40pc)

1. Truy cập [giao diện AWS Step Functions](https://console.aws.amazon.com/states/home?region=ap-southeast-1).

2. Click vào Menu để mở rộng menu của Step Functions.

![AWS Step Functions](/images/3.1.1/0001.png?featherlight=false&width=90pc)

3. Click **State machines**, sau đó click **Create state machine**.

![AWS Step Functions](/images/3.1.1/0002.png?featherlight=false&width=90pc)

4. Click chọn **Write your workflow in code**.
  + Kéo màn hình xuống phần **Definition** và thay thế nội dung bằng đoạn JSON dưới đây:

```
{
    "StartAt": "Check Name",
    "States": {
        "Check Name": {
            "Type": "Pass",
            "Next": "Check Address"
        },
        "Check Address": {
            "Type": "Pass",
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
```

![AWS Step Functions](/images/3.1.1/0003.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.1/0004.png?featherlight=false&width=90pc)

5. Chúng ta có thể thấy workflow được cập nhật như hình dưới. Click **Next** để tiếp tục.

6. Trong phần Name, đặt tên state machine là **Process_New_Account_Applications**.
  + Trong phần Permissions, chúng ta sẽ cần chỉ định IAM role cho Step Functions assume khi thực thi. Chúng ta sẽ bắt đầu với role mặc định. Click chọn **Create new role**.

![AWS Step Functions](/images/3.1.1/0005.png?featherlight=false&width=90pc)

7. Để các tùy chọn còn lại mặc định, kéo màn hình xuống dưới và click **Create state machine**.

![AWS Step Functions](/images/3.1.1/0006.png?featherlight=false&width=90pc)

{{%notice tip%}}
Trong **AWS Step Functions**, chúng tôi xác định các **state machine** của mình bằng cách sử dụng ngôn ngữ có cấu trúc dựa trên JSON được gọi là Amazon States Language. Bạn có thể đọc thêm về đặc tả ngôn ngữ đầy đủ và tất cả các loại trạng thái được hỗ trợ tại https://states-language.net/spec.html
{{%/notice%}}

8. State machine được tạo thành công.

![AWS Step Functions](/images/3.1.1/0007.png?featherlight=false&width=90pc)
+++
title = "Cập nhật workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.5.1 </b>"
+++


#### Cập nhật workflow

Các bước chúng ta sẽ làm trong phần này :
+ Thêm trạng thái  mới có tên là **Pending Review**.
+ Thêm trạng thái **Review Required ?** Sử dụng **Choice state** để chuyển sang trạng thái **Pending Review** nếu kiểm tra tên hoặc địa chỉ trả về kết quả bị gắn cờ.
+ Cập nhật bước **Check Address** để chuyển sang trạng thái **Review Required?**.

1. Quay trở lại giao diện dòng lệnh của Cloud9 instance, thay thế nội dung của file **state-machine/account-application-workflow.asl.json** với nội dung JSON dưới đây.
  + Ấn **Ctrl + S** để lưu thay đổi.

 ```
 {
    "StartAt": "Check Name",
    "States": {
        "Check Name": {
            "Type": "Task",
            "Parameters": {
                "command": "CHECK_NAME",
                "data": {
                    "name.$": "$.application.name"
                }
            },
            "Resource": "${DataCheckingFunctionArn}",
            "ResultPath": "$.checks.name",
            "Next": "Check Address"
        },
        "Check Address": {
            "Type": "Task",
            "Parameters": {
                "command": "CHECK_ADDRESS",
                "data": {
                    "address.$": "$.application.address"
                }
            },
            "Resource": "${DataCheckingFunctionArn}",
            "ResultPath": "$.checks.address",
            "Next": "Review Required?"
        },
        "Review Required?": {
            "Type": "Choice",
            "Choices": [
                {
                    "Variable": "$.checks.name.flagged",
                    "BooleanEquals": true,
                    "Next": "Pending Review"
                },
                {
                    "Variable": "$.checks.address.flagged",
                    "BooleanEquals": true,
                    "Next": "Pending Review"
                }
            ],
            "Default": "Approve Application"
        },
        "Pending Review": {
            "Type": "Pass",
            "End": true
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
 ```

![StepFunctions](/images/SF/057.png?width=90pc)

2. Thực hiện triển khai cập nhật bằng câu lệnh sau:
```
sam deploy
```

![StepFunctions](/images/SF/058.png?width=90pc)

3. Kiểm tra quá trình triển khai cập nhật thành công.

![StepFunctions](/images/SF/059.png?width=90pc)

Chúng tôi vừa thêm hai trạng thái mới vào workflow làm việc của mình: **Review Required** Và **Pending Review**. Trạng thái **Review Requried?** Kiểm tra đầu vào của nó (là đầu ra từ trạng thái **Check Address**) và chạy qua một loạt kiểm tra. 

{{%notice tip%}}
Bạn có thể thấy rằng có một loạt hai quy tắc lựa chọn trong định nghĩa của trạng thái, mỗi quy tắc chỉ định state ( trạng thái ) sẽ chuyển đến tiếp theo nếu quy tắc của nó khớp thành công. Cũng có một tên trạng thái mặc định được chỉ định để chuyển đổi sang trong trường hợp không có quy tắc phù hợp nào.
{{%/notice%}}

Một trong những quy tắc Lựa chọn của chúng ta nói rằng nếu giá trị bên trong đầu vào tại checks.name.flagged là true, thì trạng thái tiếp theo sẽ là **Pending Review**. Quy tắc lựa chọn khác cũng tương tự, kiểm tra checks.address.flagged để xem liệu nó có true hay không, trong trường hợp đó, nó cũng chuyển sang trạng thái **Pending Review**. Cuối cùng, giá trị mặc định của **Choice state** của chúng ta cho biết rằng nếu không có quy tắc lựa chọn nào phù hợp, thì state machine sẽ chuyển sang trạng thái **Approve Application**.

Bước tiếp theo chúng ta sẽ thử thực thi workflow để xem kết quả.


{{%notice info%}}
Nếu bạn quan tâm nhiều tới hành vi và các loại so sánh được hỗ trợ bởi **Choice state**, hãy xem hướng dẫn dành cho nhà phát triển tại [https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-choice-state.html](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-choice-state.html)
{{%/notice%}}


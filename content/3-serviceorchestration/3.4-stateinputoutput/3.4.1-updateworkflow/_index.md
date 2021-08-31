+++
title = "Cập nhật workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.4.1 </b>"
+++


#### Cập nhật workflow

Trong bước này chúng ta sẽ thêm thuộc tính ResultPath vào trạng thái **Check Name** và **Check Address** bên trong state machine của chúng ta được xác định trong **state-machine / account-application-workflow.asl.json**.

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
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
 ```

![StepFunctions](/images/SF/049.png?width=90pc)

2. Thực hiện triển khai cập nhật bằng câu lệnh sau:
```
sam deploy
```

![StepFunctions](/images/SF/050.png?width=90pc)

3. Kiểm tra quá trình triển khai cập nhật thành công.

![StepFunctions](/images/SF/051.png?width=90pc)

Bước tiếp theo chúng ta sẽ thử thực thi workflow để xem kết quả.


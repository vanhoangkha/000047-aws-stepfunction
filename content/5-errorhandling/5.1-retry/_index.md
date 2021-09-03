+++
title = "Thêm tính năng retry"
date = 2021
weight = 1
chapter = false
pre = "<b>5.1 </b>"
+++


#### Thêm tính năng retry

Các Task state (và các trạng thái khác như Parallel state, mà chúng ta sẽ nói đến sau), có khả năng thử lại công việc  khi gặp lỗi. Chúng ta chỉ cần thêm tham số Thử lại vào định nghĩa Task state của mình, cho state machine biết loại lỗi nào  nên thử lại và tùy chọn chỉ định cấu hình bổ sung để kiểm soát tốc độ thử lại và số lần thử lại tối đa.


1. Quay trở lại giao diện dòng lệnh của Cloud9 instance, thay thế nội dung của file **statemachine/account-application-workflow.asl.json** với nội dung dưới đây.

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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
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
            "Type": "Task",
            "Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken",
            "Parameters": {
                "FunctionName": "${FlagApplicationFunctionName}",
                "Payload": {
                    "id.$": "$.application.id",
                    "flagType": "REVIEW",
                    "taskToken.$": "$$.Task.Token"
                }
            },
            "ResultPath": "$.review",
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
            "Next": "Review Approved?"
        },
        "Review Approved?": {
            "Type": "Choice",
            "Choices": [
                {
                    "Variable": "$.review.decision",
                    "StringEquals": "APPROVE",
                    "Next": "Approve Application"
                },
                {
                    "Variable": "$.review.decision",
                    "StringEquals": "REJECT",
                    "Next": "Reject Application"
                }
            ]
        },
        "Reject Application": {
            "Type": "Task",
            "Parameters": {
                "id.$": "$.application.id"
            },
            "Resource": "${RejectApplicationFunctionArn}",
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
            "End": true
        },
        "Approve Application": {
            "Type": "Task",
            "Parameters": {
                "id.$": "$.application.id"
            },
            "Resource": "${ApproveApplicationFunctionArn}",
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
            "End": true
        }
    }
}
```

![StepFunctions](/images/SF/093.png?width=90pc)

2. Chạy lệnh dưới đây để thực hiện deploy, kiểm tra quá trình deploy thành công trước khi làm bước tiếp theo:
```
sam deploy
```
![StepFunctions](/images/SF/094.png?width=90pc)

{{%notice tip%}}
Chúng ta có thể chỉ định cấu hình bổ sung cho các thông số Thử lại của mình, bao gồm **IntervalSeconds** (mặc định là 1), **MaxAttempts** (mặc định là 3) và **BackoffRate** (mặc định là 2), các cấu hình mặc định cũng tương đối phù hợp với trường hợp của chúng ta, vì vậy chúng ta vẫn sử dụng các giá trị mặc định.
{{%/notice%}}

Hiện tại, chúng ta không kiểm tra thử lỗi được bởi vì việc Lambda function sinh ra exception chỉ là tạm thời. Nhưng mục đích của bước này giúp cung cấp các ví dụ để bạn biết cách tự thêm các kiểu thử lại cho state machine của bạn. Tiếp tục, chúng ta cũng hãy tìm hiểu cách xử lý các lỗi cấp ứng dụng cụ thể.
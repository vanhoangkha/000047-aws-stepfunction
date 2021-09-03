+++
title = "Thêm tính năng catch"
date = 2021
weight = 2
chapter = false
pre = "<b>5.2 </b>"
+++


#### Thêm tính năng catch

Ngoài việc xử lý các vấn đề tạm thời với Thử lại, Step Functions còn cho phép chúng ta bắt các lỗi cụ thể và phản hồi bằng cách chuyển sang các trạng thái thích hợp để xử lý các lỗi này. 

Ví dụ: giả sử rằng có một số loại tên mà dịch vụ kiểm tra dữ liệu của chúng ta không thể xử lý. Trong những trường hợp này, thay vì gắn cờ đơn đăng ký để xem xét, chúng ta sẽ  gắn cờ đơn đăng ký theo dạng **không thể xử lý được** do dữ liệu không tương thích.

Để minh họa cho khả năng này, chúng ta sẽ tận dụng một số mã kiểm tra trong Lambda function **Data Checking** để yêu cầu nó phát ra lỗi nếu nó thấy một chuỗi kí tự cụ thể đi qua trong tên của người đăng ký. 

Chúng tôi sẽ cập nhật state machine của mình để phát hiện loại lỗi tùy chỉnh cụ thể này và chuyển hướng đến trạng thái mới, đồng thời gắn cờ đơn đăng ký là **không thể xử lý**.

Trong bước này chúng ta sẽ:
  + Thêm một trạng thái mới là **Flag Application As Unprocessable** vào state machine.

  + Thêm cấu hình Catch vào trạng thái **Check Name** trong state machine của chúng ta,  nếu gặp dữ liệu không thể xử lý thì sẽ chuyển đổi sang trạng thái **Flag Application As Unprocessable**.
  
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
            "Catch": [
                {
                    "ErrorEquals": [
                        "UnprocessableDataException"
                    ],
                    "ResultPath": "$.error-info",
                    "Next": "Flag Application As Unprocessable"
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
        },
        "Flag Application As Unprocessable": {
            "Type": "Task",
            "Resource": "arn:aws:states:::lambda:invoke",
            "Parameters": {
                "FunctionName": "${FlagApplicationFunctionName}",
                "Payload": {
                    "id.$": "$.application.id",
                    "flagType": "UNPROCESSABLE_DATA",
                    "errorInfo.$": "$.error-info"
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
            "End": true
        }
    }
}
```

![StepFunctions](/images/SF/095.png?width=90pc)

2. Chạy lệnh dưới đây để thực hiện deploy, kiểm tra quá trình deploy thành công trước khi làm bước tiếp theo:
```
sam deploy
```
![StepFunctions](/images/SF/096.png?width=90pc)

3. Chạy lệnh dưới đây để  gửi đơn đăng ký mới có chứa dữ liệu không thể xử lý được cho trường tên của người đăng ký.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "UNPROCESSABLE_DATA", "address": "123 Street" }' /dev/stdout 
```
![StepFunctions](/images/SF/097.png?width=90pc)


4. Quay trở lại giao diện Step Functions, Click vào tên state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.

![StepFunctions](/images/SF/098.png?width=90pc)

5. Click vào lần thực thi mới nhất.
![StepFunctions](/images/SF/099.png?width=90pc)

6. Chúng ta có thể nhìn thấy state machine của chúng ta đã phát hiện dữ liệu không thể xử lý và chuyển trạng thái tới **Flag Application As Unprocessable**.

![StepFunctions](/images/SF/100.png?width=90pc)

7. Chúng ta có thể kiểm tra đơn đăng ký bị gắn cờ không thể xử lý bằng cách chạy lệnh dưới đây.
```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_WITH_UNPROCESSABLE_DATA" }' /dev/stdout 
```
![StepFunctions](/images/SF/101.png?width=90pc)

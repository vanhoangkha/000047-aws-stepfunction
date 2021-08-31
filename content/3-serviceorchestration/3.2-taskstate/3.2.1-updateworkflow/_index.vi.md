+++
title = "Update workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.2.1 </b>"
+++


#### Update workflow

1. Truy cập [giao diện AWS Step Functions](https://console.aws.amazon.com/states/home?region=ap-southeast-1).
  + Click chọn **Process_New_Account_Applications**.
  + Click **Edit**.

![StepFunctions](/images/SF/031.png?width=90pc)

2. Tiếp theo, chúng ta sẽ cập nhật định nghĩa của state machine. Lưu ý rằng sau khi dán nội dung bên dưới, bạn sẽ thấy một vài dòng có chỉ báo lỗi vì định nghĩa máy trạng thái mới của chúng tôi có một số chuỗi giữ chỗ được ghi là ‘REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN’. **Chúng ta sẽ khắc phục sự cố này trong bước tiếp theo**. Thay thế định nghĩa hiện có  bằng định nghĩa  sau:
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
            "Resource": "REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN",
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
            "Resource": "REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN",
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}

```
![StepFunctions](/images/SF/032.png?width=90pc)

3. Quay trở lại giao diện Terminal của Cloud9, chạy lệnh dưới đây, để xác định được **ARN** của **DataCheckingFunction**. ( Đảm bảo rằng bạn đang ở vị trí thư mục **workshop-dir**. )

```
REGION=$(grep region samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
STACK_NAME=$(grep stack_name samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
aws cloudformation describe-stacks --region $REGION --stack-name $STACK_NAME --query 'Stacks[0].Outputs[?OutputKey==`DataCheckingFunctionArn`].OutputValue' --output text                
```
{{%notice tip%}}
Bộ lệnh có vẻ phức tạp trên chỉ để giúp bạn lấy ARN nhahn hơn bằng cách tự động lấy các giá trị cấu hình ra khỏi file samconfig.toml (file này ghi nhớ những thứ như Region và tên cloudformation stack mà chúng ta đang sử dụng SAM để triển khai), sau đó sử dụng các cấu hình đó để cấu hình giá trị cho lệnh AWS CLI để hiển thị ARN của Lambda functions **DataCheckingFunction** chúng ta đã triển khai.
{{%/notice%}}

![StepFunctions](/images/SF/033.png?width=90pc)

4. Copy và lưu lại kết quả của bước 3.
5. Trong mục **Definition** của state machine, cập nhật thông số ARN của Lambda function của bước 3 như hình dưới :
  + Click **Save**.

![StepFunctions](/images/SF/034.png?width=90pc)

6. Chúng ta nhận được cảnh báo rằng  IAM role của chúng ta có thể cần thay đổi để cho phép state machine thực thi. Đây là một lời nhắc nhở hữu ích. Trên thực tế, chúng ta đã thay đổi state machine của mình và sẽ yêu cầu thay đổi quyền. Bây giờ, chúng ta yêu cầu khả năng Lambda function **Data Checking**. Chúng ta sẽ giải quyết vấn đề này ở bước lớn tiếp theo.Click **Save anyway**.
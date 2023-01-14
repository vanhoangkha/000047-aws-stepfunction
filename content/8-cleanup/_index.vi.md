+++
title = "Dọn dẹp tài nguyên"
date = 2021-07-09T14:40:59+07:00
weight = 8
chapter = false
pre = "<b>8. </b>"
+++

Bạn sẽ dọn dẹp tài nguyên theo thứ tự sau:

1. Truy cập vào giao diện quản lý của [AWS Step Functions](https://console.aws.amazon.com/states/home?region=ap-southeast-1).
  + Click vào state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
  + Click vào execution đang running.

![AWS Step Functions](/images/8/0001.png?featherlight=false&width=90pc)

1. Click **Stop execution** để dừng thực thi state machine.

![AWS Step Functions](/images/8/0002.png?featherlight=false&width=90pc)

{{%notice warning%}}
Đảm bảo không còn execution nào đang ở trạng thái **running** trước khi bạn thực hiện bước tiếp theo. AWS Cloud Formation sẽ không thực hiện xóa state machine nếu như vẫn còn execution đang running.
{{%/notice%}}

1. Quay trở lại giao diện Cloud9 Instance. chạy lệnh sau để thực hiện xóa AWS CloudFormation stack mà AWS SAM đã tạo cho ứng dụng của chúng ta.
```
REGION=$(grep region samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
STACK_NAME=$(grep stack_name samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
aws cloudformation delete-stack --region $REGION --stack-name $STACK_NAME
```
![AWS Step Functions](/images/8/0003.png?featherlight=false&width=90pc)

1. Truy cập vào giao diện quản lý [dịch vụ Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9/home?region=ap-southeast-1)
  + Chọn Cloud9 Instance **StepFunctions**.
  + Click **Delete**.

![AWS Step Functions](/images/8/0004.png?featherlight=false&width=90pc)

2. Điền **Delete** để xác nhận.
  + Click **Delete** để tiến hành xóa Cloud9 Instance.

![AWS Step Functions](/images/8/0005.png?featherlight=false&width=90pc)
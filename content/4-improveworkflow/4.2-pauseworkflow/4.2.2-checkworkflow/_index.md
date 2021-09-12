+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>4.2.2 </b>"
+++


#### Kiểm tra workflow
Giờ đây, chúng ta có thể gửi đơn đăng ký không hợp lệ, đơn đăng ký sẽ được gắn cờ để xem xét, phê duyệt hoặc từ chối bởi kiểm soát viên và sau đó kết quả xem xét sẽ được trả về state machine để tiếp tục thực thi tiếp qui trình.

1. Chạy câu lệnh dưới để thực hiện submit một đăng ký có địa chỉ không hợp lệ.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "InvalidAddressFormat" }' /dev/stdout 
```
![StepFunctions](/images/SF/082.png?width=90pc)

2. Kiểm tra xem đăng ký của chúng ta có bị gắn cờ để kiểm tra lại hay không bằng lệnh dưới đây
```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_FOR_REVIEW" }' /dev/stdout 

```

![StepFunctions](/images/SF/083.png?width=90pc)

3. Quét khối và copy id của đơn đăng ký bị gắn cờ.

![StepFunctions](/images/SF/084.png?width=90pc)


4. Quay lại giao diện dịch vụ Step Functions , click vào State machine **ApplicationProcessingStateMachine-xxxxxxx**.

![StepFunctions](/images/SF/085.png?width=90pc)

5. Tại tab Executions, chúng ta có thể thấy trạng thái của lần thực thi này đang thể hiện là **Running**. Click chọn lần thực thi mới nhất.
  + Chúng ta sẽ thấy  trạng thái **Pending Review** đang **in-progress**. Điều này có nghĩa là state machine của chúng ta đang tạm dừng và chờ phản hồi trước khi tiếp tục thực thi.

![StepFunctions](/images/SF/086.png?width=90pc)

6. Quay trở lại Cloud9 console, chúng ta sẽ cần phản hồi và thực hiện approve đơn đăng ký. ( Hiện chúng ta chưa xây dựng giao diện web cho việc này, chúng ta chỉ gọi 1 Lambda Function trong dịch vụ **Account Applications** để thực hiện thao tác approve.)
  + Chạy câu lệnh dưới đây và thay thế **REPLACE_WITH_APPLICATION_ID** với giá trị ID đăng ký bạn đã copy lại ở bước 3.

```
aws lambda invoke --function-name sfn-workshop-ReviewApplication --payload '{ "id": "REPLACE_WITH_APPLICATION_ID", "decision": "APPROVE" }' /dev/stdout 
```

![StepFunctions](/images/SF/087.png?width=90pc)

7. Quay trở lại giao diện Step Functions chúng ta có thể thấy việc thực thi đã được tiếp tục và hoàn tất ( do chúng ta đã approve đơn đăng ký).
  + Click vào state **Review Approved?**.
  + Click vào **Step input**.
  + Chúng ta có thể thấy quyết định approve đã được đưa vào phần dữ liệu input. (thông qua phản hồi  **SendTaskSuccess** mà  Lambda function **Review Application** - functions/account-Applications/review.js đã gọi).

![StepFunctions](/images/SF/088.png?width=90pc)

Để hoàn tất việc triển khai quy trình làm việc, chúng ta sẽ thay thế các bước **Approve Application** và **Reject Application** để gọi các Lambda function ApproveApplication và RejectApplication mà chúng ta đã tạo.
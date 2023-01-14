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
![AWS Step Functions](/images/4.2.2/0001.png?featherlight=false&width=90pc)

1. Kiểm tra xem đăng ký của chúng ta có bị gắn cờ để kiểm tra lại hay không bằng lệnh dưới đây
```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_FOR_REVIEW" }' /dev/stdout 

```
![AWS Step Functions](/images/4.2.2/0002.png?featherlight=false&width=90pc)

1. Quét khối và copy id của đơn đăng ký bị gắn cờ.

![AWS Step Functions](/images/4.2.2/0003.png?featherlight=false&width=90pc)


2. Quay lại giao diện dịch vụ Step Functions , click vào State machine **ApplicationProcessingStateMachine-xxxxxxx**.

![AWS Step Functions](/images/4.2.2/0004.png?featherlight=false&width=90pc)

3. Tại tab Executions, chúng ta có thể thấy trạng thái của lần thực thi này đang thể hiện là **Running**. Click chọn lần thực thi mới nhất.
  + Chúng ta sẽ thấy  trạng thái **Pending Review** đang **in-progress**. Điều này có nghĩa là state machine của chúng ta đang tạm dừng và chờ phản hồi trước khi tiếp tục thực thi.

![AWS Step Functions](/images/4.2.2/0005.png?featherlight=false&width=90pc)

4. Quay trở lại Cloud9 console, chúng ta sẽ cần phản hồi và thực hiện approve đơn đăng ký. ( Hiện chúng ta chưa xây dựng giao diện web cho việc này, chúng ta chỉ gọi 1 Lambda Function trong dịch vụ **Account Applications** để thực hiện thao tác approve.)
  + Chạy câu lệnh dưới đây và thay thế **REPLACE_WITH_APPLICATION_ID** với giá trị ID đăng ký bạn đã copy lại ở bước 3.

```
aws lambda invoke --function-name sfn-workshop-ReviewApplication --payload '{ "id": "REPLACE_WITH_APPLICATION_ID", "decision": "APPROVE" }' /dev/stdout 
```

![AWS Step Functions](/images/4.2.2/0006.png?featherlight=false&width=90pc)

1. Quay trở lại giao diện Step Functions chúng ta có thể thấy việc thực thi đã được tiếp tục và hoàn tất ( do chúng ta đã approve đơn đăng ký).
  + Click vào state **Review Approved?**.
  + Click vào **Step input**.
  + Chúng ta có thể thấy quyết định approve đã được đưa vào phần dữ liệu input. (thông qua phản hồi  **SendTaskSuccess** mà  Lambda function **Review Application** - functions/account-Applications/review.js đã gọi).

![AWS Step Functions](/images/4.2.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/0009.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/00010.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/00011.png?featherlight=false&width=90pc)

Để hoàn tất việc triển khai quy trình làm việc, chúng ta sẽ thay thế các bước **Approve Application** và **Reject Application** để gọi các Lambda function ApproveApplication và RejectApplication mà chúng ta đã tạo.
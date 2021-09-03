+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>6.2 </b>"
+++


#### Kiểm tra workflow

Chúng ta sẽ thử gửi các đơn đăng ký khác nhau để xem state machine của chúng ta thực thi như thế nào.

1. Chạy câu lệnh dưới để thực hiện submit một đăng ký  hợp lệ.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "123 Enterprise Street" }' /dev/stdout 
```
**ApplicationProcessingStateMachine-xxxxxxxxxxxx**/

![StepFunctions](/images/SF/104.png?width=90pc)

2. Quay lại giao diện StepFunctions, click vào state machine 
![StepFunctions](/images/SF/105.png?width=90pc)

3. Tại tab Executions, click vào lần thực thi mới nhất.
![StepFunctions](/images/SF/106.png?width=90pc)


4. Chúng ta có thể thấy các state **Check Name** và **Check Address** được thực thi song song với nhau.

![StepFunctions](/images/SF/107.png?width=90pc)

5. Chạy câu lệnh dưới để thực hiện submit một đăng ký không hợp lệ.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "ABadAddress" }' /dev/stdout 
```
![StepFunctions](/images/SF/108.png?width=90pc)

6. Làm tương tự bước 2-4, chúng ta sẽ thấy state machine của chúng ta dừng ở bước **Pending Review**.

![StepFunctions](/images/SF/109.png?width=90pc)

7.  Chạy câu lệnh dưới để thực hiện submit một đăng ký có dữ liệu không thể xử lý.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "UNPROCESSABLE_DATA", "address": "123 Street" }' /dev/stdout 
```
![StepFunctions](/images/SF/110.png?width=90pc)

8. Làm tương tự bước 2-4, chúng ta sẽ thấy state machine của chúng ta dừng ở bước **Flag Application as Unprocessable**.
![StepFunctions](/images/SF/111.png?width=90pc)

Chúc mừng bạn đã hoàn thành workshop, chúng ta có một state machine có cấu trúc tốt để quản lý quy trình xử lý các đăng ký mới cho hệ thống ngân hàng đơn giản. Nếu muốn, chúng ta có thể thêm một bước nữa trong quy trình làm việc của mình để xử lý logic chi tiết hơn nữa liên quan đến việc mở tài khoản ngân hàng cho các đơn đăng ký được chấp thuận. Hi vọng qua workshop này bạn đã có thêm kinh nghiệm cần thiết để tiếp tục tự mình phát triển ứng dụng với AWS Step Functions.
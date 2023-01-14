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

![AWS Step Functions](/images/6.2/0001.png?featherlight=false&width=90pc)

1. Quay lại giao diện StepFunctions, click vào state machine 

![AWS Step Functions](/images/6.2/0002.png?featherlight=false&width=90pc)

2. Tại tab Executions, click vào lần thực thi mới nhất.

![AWS Step Functions](/images/6.2/0003.png?featherlight=false&width=90pc)


3. Chúng ta có thể thấy các state **Check Name** và **Check Address** được thực thi song song với nhau.

![AWS Step Functions](/images/6.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0006.png?featherlight=false&width=90pc)

4. Chạy câu lệnh dưới để thực hiện submit một đăng ký không hợp lệ.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "ABadAddress" }' /dev/stdout 
```
![AWS Step Functions](/images/6.2/0007.png?featherlight=false&width=90pc)

1. Làm tương tự bước 2-4, chúng ta sẽ thấy state machine của chúng ta dừng ở bước **Pending Review**.

![AWS Step Functions](/images/6.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0009.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/00010.png?featherlight=false&width=90pc)

2.  Chạy câu lệnh dưới để thực hiện submit một đăng ký có dữ liệu không thể xử lý.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "UNPROCESSABLE_DATA", "address": "123 Street" }' /dev/stdout 
```

![AWS Step Functions](/images/6.2/00011.png?featherlight=false&width=90pc)

1. Làm tương tự bước 2-4, chúng ta sẽ thấy state machine của chúng ta dừng ở bước **Flag Application as Unprocessable**.

![AWS Step Functions](/images/6.2/00012.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/00013.png?featherlight=false&width=90pc)

Chúc mừng bạn đã hoàn thành workshop, chúng ta có một state machine có cấu trúc tốt để quản lý quy trình xử lý các đăng ký mới cho hệ thống ngân hàng đơn giản. Nếu muốn, chúng ta có thể thêm một bước nữa trong quy trình làm việc của mình để xử lý logic chi tiết hơn nữa liên quan đến việc mở tài khoản ngân hàng cho các đơn đăng ký được chấp thuận. Hi vọng qua workshop này bạn đã có thêm kinh nghiệm cần thiết để tiếp tục tự mình phát triển ứng dụng với AWS Step Functions.
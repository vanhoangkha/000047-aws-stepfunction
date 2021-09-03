+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>4.1.2 </b>"
+++


#### Kiểm tra workflow


1. Chạy câu lệnh dưới đây để thực thi Lambda function gửi đơn đăng ký chứa thông tin địa chỉ không hợp lệ.

 ```
 aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "AnInvalidAddress" }' /dev/stdout 
 ```
![StepFunctions](/images/SF/071.png?width=90pc)

2. Truy cập vào [giao diện State machines](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click vào state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.

![StepFunctions](/images/SF/072.png?width=90pc)

3. Tại tab **Executions**, click vào lần thực thi mới nhất.

![StepFunctions](/images/SF/073.png?width=90pc)

4. Click state **Pending Review**, sau đó click **Step output**.
  + Chúng ta có thể thấy thuộc tính ID của đơn đăng ký đã được thêm vào.

![StepFunctions](/images/SF/074.png?width=90pc)

Vậy là chúng ta đã chuyển ID đơn đăng ký vào Step Functions thành công, chúng ta đã sẵn sàng để cấu hình cho state **Pending Review** thông báo cho dịch vụ **Account Applications** khi có đơn đăng ký bị gắn cờ và sẽ tạm dừng việc thực thi state machine để chờ cho kiểm soát viên kiểm tra lại thông tin đăng ký.
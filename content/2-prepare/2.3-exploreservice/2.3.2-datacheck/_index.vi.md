+++
title = "Data Checking"
date = 2021
weight = 2
chapter = false
pre = "<b>2.3.2 </b>"
+++

#### Kiểm tra dịch vụ **Data Checking**.

1. Chạy câu lệnh dưới đây trong giao diện dòng lệnh để kiểm tra tên có phù hợp hay không.
```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_NAME", "data": { "name": "Spock" } }' /dev/stdout
```
  + Kết quả trả về ** {"flagged":false}** , như vậy tên này phù hợp.

![AWS Step Functions](/images/2.3.2/0001.png?featherlight=false&width=90pc)

2. Chạy câu lệnh dưới đây, lần này tên chúng ta sẽ cố tính đặt không phù hợp.

```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_NAME", "data": { "name": "evil Spock" } }' /dev/stdout
```
  + Kết quả trả về **{"flagged":true}**, như vậy tên này không phù hợp.

![AWS Step Functions](/images/2.3.2/0002.png?featherlight=false&width=90pc)

3. Chạy câu lệnh dưới đây trong giao diện dòng lệnh để kiểm tra địa chỉ có phù hợp hay không.
```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_ADDRESS", "data": { "address": "123 Street" } }' /dev/stdout
```
  + Kết quả trả về ** {"flagged":false}** , như vậy địa chỉ này phù hợp.

![AWS Step Functions](/images/2.3.2/0003.png?featherlight=false&width=90pc)

4. Chạy câu lệnh dưới đây, lần này tên chúng ta sẽ cố tính đặt không phù hợp.

```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_ADDRESS", "data": { "address": "DoesntMatchAddressPattern" } }' /dev/stdout
```
  + Kết quả trả về **{"flagged":true}**, như vậy địa chỉ này không phù hợp.

![AWS Step Functions](/images/2.3.2/0004.png?featherlight=false&width=90pc)

Như bạn có thể thấy, dịch vụ **Data Checking** chỉ trả về một phản hồi kiểu JSON đơn giản với một biến, được gắn cờ trả về true nếu giá trị cần kiểm soát viên kiểm tra lại.

Hiện tại, chúng ta đã có tất cả các khả năng cơ bản cần có trong các dịch vụ của mình để bắt đầu kết nối chúng với nhau nhằm triển khai các bước khởi đầu của quy trình xử lý đơn đăng ký. 

Vấn đề của chúng ta là  "làm thế nào để kết nối các dịch vụ này với nhau để triển khai thành quy trình làm việc chúng ta muốn?" 
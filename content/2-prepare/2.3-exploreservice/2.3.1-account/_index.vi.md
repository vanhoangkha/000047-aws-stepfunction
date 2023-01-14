+++
title = "Account Application"
date = 2021
weight = 1
chapter = false
pre = "<b>2.3.1 </b>"
+++

#### Kiểm tra dịch vụ **Account Application**.

Sử dụng AWS SAM CLI, chúng tôi có thể gọi bất kỳ Lambda Function nào trong dịch vụ của mình với các tham số tùy thuộc vào những gì chúng tôi muốn làm. Hãy thử chạy lần lượt từng lệnh này để hiểu những gì chúng ta có thể làm với các đơn đăng ký hoặc dữ liệu người dùng.

Dưới đây, chúng ta đang sử dụng AWS CLI, thông qua **aws lambda invoke**, để gọi trực tiếp một  AWS Lambda Function đã triển khai với nội dung mong muốn. Chúng ta vẫn chưa bắt đầu sử dụng chức năng của AWS Step Functions. Ở đây, chúng ta chỉ đang kiểm tra các  Lambda Function mà chúng ta sẽ bắt đầu sắp xếp thành workflow bằng cách sử dụng  StepFunctions trong workshop này.

1. Chạy câu lệnh dưới đây trong giao diện dòng lệnh để gửi một đăng ký mới.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "123 Enterprise Street" }' /dev/stdout 
```
![AWS Step Functions](/images/2.3.1/0001.png?featherlight=false&width=90pc)

2. Copy ID của đơn đăng ký, trong kết quả của câu lệnh trên để sử dụng cho bước tiếp theo.
  + Ví dụ : **application_869a2256-fe54-4731-9522-4cbbc1a184ad**

3. Gắn cờ một đăng ký để được kiểm tra bởi kiểm soát viên. ( lưu ý thay thế **REPLACE_WITH_ID** bằng giá trị ID của đơn đăng ký bạn đã lưu lại ở bước thứ 2.)
```
aws lambda invoke --function-name sfn-workshop-FlagApplication --payload '{ "id": "REPLACE_WITH_ID", "flagType": "REVIEW" }' /dev/stdout
```
  + Ví dụ lệnh sau khi thay đổi REPLACE_WITH_ID.
  ```
  aws lambda invoke --function-name sfn-workshop-FlagApplication --payload '{ "id": "application_869a2256-fe54-4731-9522-4cbbc1a184ad", "flagType": "REVIEW" }' /dev/stdout
  ```

![AWS Step Functions](/images/2.3.1/0002.png?featherlight=false&width=90pc)

4. Liệt kê tất cả đăng ký hiện đang được gắn cờ để review.

```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_FOR_REVIEW" }' /dev/stdout
```

![AWS Step Functions](/images/2.3.1/0003.png?featherlight=false&width=90pc)

5. Thực hiện Approve một đơn đăng ký bị gắn cờ:( lưu ý thay thế **REPLACE_WITH_ID** bằng giá trị ID của đơn đăng ký bạn đã lưu lại ở bước thứ 2.)

```
aws lambda invoke --function-name sfn-workshop-ApproveApplication --payload '{ "id": "REPLACE_WITH_ID" }' /dev/stdout
```
  + Ví dụ lệnh sau khi thay đổi REPLACE_WITH_ID.
  ```
  aws lambda invoke --function-name sfn-workshop-ApproveApplication --payload '{ "id": "application_869a2256-fe54-4731-9522-4cbbc1a184ad" }' /dev/stdout
  ```

![AWS Step Functions](/images/2.3.1/0004.png?featherlight=false&width=90pc)

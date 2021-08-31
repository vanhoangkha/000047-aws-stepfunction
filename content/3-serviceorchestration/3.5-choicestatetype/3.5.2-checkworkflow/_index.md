+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.5.2 </b>"
+++


#### Kiểm tra workflow


1. Quay trở lại giao diện Step Functions.
  + Click **New execution**.
 
![StepFunctions](/images/SF/060.png?width=90pc)

2. Thử tạo 1 đơn đăng ký hợp lệ bằng cách sử dụng input json như dưới đây:
```
{ "application": { "name": "Spock", "address": "123 Enterprise Street" } }
```
  + Click **Start execution**.

![StepFunctions](/images/SF/061.png?width=90pc)

3. Kiểm tra trạng thái **Review Required?** đã chuyển sang **Approve Application** , vì cả tên v2a địa chỉ chứa thông tin hợp lệ.

![StepFunctions](/images/SF/062.png?width=90pc)


4. Click **New execution** để thực hiện một thực thi mới.

![StepFunctions](/images/SF/063.png?width=90pc)

5. Thử tạo 1 đơn đăng ký không hợp lệ bằng cách sử dụng input json như dưới đây:
```
{ "application": { "name": "Spock", "address": "Somewhere" } }
```
  + Click **Start execution**.

![StepFunctions](/images/SF/064.png?width=90pc)

6. Hãy lưu ý cách chúng ta định tuyến đến **Pending Review**, lần này chúng ta chuyển đến một địa chỉ không hợp lệ theo mẫu <số>-<khoảng trắng>-<chữ>. ( ví dụ : 123 Street) 

![StepFunctions](/images/SF/065.png?width=90pc)

Nhờ **Choice state**, chúng ta hiện đang định tuyến workflow của mình theo cách chúng ta muốn. Tuy nhiên, chúng ta vẫn chưa thực hiện xử lý gì ở state **Approve Application** và **Pending Review**. Chúng tôi sẽ tạm dừng việc triển khai bước Phê duyệt đơn đăng ký cho đến phần sau của workshop (chúng ta đã biết cách tích hợp với một hàm Lambda, vì vậy đó chưa phải là bước tiếp theo thú vị để thực hiện). Thay vào đó, chúng tôi sẽ tiếp tục duy trì đà học tập và tìm hiểu cách triển khai trạng thái **Pending Review** trong bước kế tiếp.
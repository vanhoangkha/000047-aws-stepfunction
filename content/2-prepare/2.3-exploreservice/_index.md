+++
title = "Kiểm tra dịch vụ mẫu"
date = 2021
weight = 3
chapter = false
pre = "<b>2.3 </b>"
+++


#### Kiểm tra dịch vụ mẫu - Tổng quan

Trong phần này, chúng ta sẽ tương tác theo cách thủ công với từng Lambda function  để hiểu dịch vụ **Account Application** và API dịch vụ **Data Checking** của chúng ta.

Các dịch vụ này được triển khai dưới dạng tập hợp các hàm AWS Lambda. Dịch vụ **Account Application** cung cấp cho chúng ta các tính năng:

 + Gửi đơn đăng ký mới.
 + Xem danh sách các đăng ký theo từng trạng thái.
 + Gắn cờ một đăng ký để xem xét.
 + Phê duyệt hoặc từ chối đơn đăng ký

Dịch vụ **Data Checking** cho phép chúng ta:

 + Kiểm tra một cái tên để đảm bảo nó hợp lệ.
 + Xác thực một địa chỉ phù hợp với một qui tắc cụ thể.

{{%notice tip%}}
Mặc dù dịch vụ **Account Application** có khả năng cho phép gắn cờ một đăng ký, nhưng chúng ta vẫn chưa tạo ra logic để xác định khi nào một đăng ký có thể bị gắn cờ. \
Chúng ta chỉ đang thiết lập các khả năng cơ bản để điều phối cùng với dịch vụ **Data Checking**.
{{%/notice%}}

#### Nội dung
1. [Kiểm tra dịch vụ **Account Application**.](2.3.1-account/)
2. [Kiểm tra dịch vụ **Data Checking**.](2.3.2-datacheck/)

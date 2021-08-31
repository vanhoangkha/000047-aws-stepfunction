+++
title = "Lĩnh vực kinh doanh"
date = 2021
weight = 2
chapter = false
pre = "<b>1.2 </b>"
+++


#### Lĩnh vực kinh doanh ( Business domain )

Sẽ dễ dàng hơn khi tìm hiểu về dịch vụ điều phối khi chúng ta thiết lập workflow dựa trên một lĩnh vực kinh doanh cụ thể. Trong workshop này chúng ta sẽ xoay quanh một số các dịch vụ ( hoạt động dưới dạng những Lambda Functions đơn giản), giả lập một quy trình đơn giản hóa trong lĩnh vực ngân hàng ( banking ). Workshop sẽ tạo ra một workflow để quản lý qui trình tạo tài khoản ngân hàng mới bằng cách kiểm tra một số thông tin của người đăng ký tự động cũng như kết hợp với kiểm soát viên.

Hệ thống mẫu của chúng ta sẽ bao gồm:
 + Dịch vụ **Account Applications**: Xử lý các đơn đăng ký mới.Yêu cầu trả lời từ dịch vụ **Data Checking** để cung cấp việc kiểm tra thông tin tên và địa chỉ người đăng ký.
 + Dịch vụ **Data Checking**: Kiểm tra thông tin tên và địa chỉ người đăng ký.
 + Dịch vụ **Accounts**: chịu trách nhiệm mở và vận hành tài khoản ngân hàng, trong workshop này chúng ta sẽ không triển khai hoàn chỉnh dịch vụ này mà tập trung vào 2 dịch vụ **Account Applications** và **Data Checking**. Dịch vụ **Accounts** sẽ là đích đến khi đơn đăng ký mở tài khoản mới được duyệt.

Trong hình dưới đây chúng ta có thể thấy được các bước trong quy trình đăng ký tài khoản , và chúng ta sẽ dùng dịch vụ điều phối để tạo workflow chi tiết cho các bước này:

![StepFunctions](/images/SF/001.png?width=60pc)

+++
title = "Dọn dẹp tài nguyên"
date = 2021-07-09T14:40:59+07:00
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

Bạn sẽ dọn dẹp tài nguyên theo thứ tự sau:
1. Xóa EC2 Instance
    - Truy cập [EC2 Management Console](console.aws.amazon.com/ec2)
    - Trên thanh điều hướng bên trái, chọn **Intances**.
    - Chọn tất cả EC2 Instance liên quan tới bài lab (**bạn có thể sử dụng thẻ để lọc các instance cần xóa hoặc tham khảo Resource Group được tạo**)
    - Click **Actions**.
    - Click **Manage Instance State**.
    - Click chọn **Terminate**.
    - Click **Change State** , sau đó click **Terminate**.

![Tag](/images/tag/023.png?width=90pc)

2. Xóa Resource Group
    - Truy cập vào [AWS Resource Group Console](console.aws.amazon.com/resource-groups/).
    - Click **Saved Resource Group** ở thanh bên trái.
    - Click vào tên Resource Group liên quan tới bài lab ( **MarketingBU** ).
    - Click **Delete**, sau đó click **Delete** một lần nữa để xác nhận xóa Resource Group.

![Tag](/images/tag/024.png?width=90pc)
 
+++
title = "Tạo Cloud9 Instance"
date = 2021
weight = 1
chapter = false
pre = "<b>2.1 </b>"
+++


#### Tạo Cloud9 Instance

AWS Cloud9 là một môi trường phát triển tích hợp (IDE) cho phép bạn viết, chạy và gỡ lỗi mã của mình trên trình duyệt. Cloud9 bao gồm một trình soạn thảo mã, trình gỡ lỗi và giao diện dòng lệnh. Cloud9 được đóng gói sẵn với các công cụ cần thiết cho các ngôn ngữ lập trình phổ biến, bao gồm JavaScript, Python, PHP, v.v..

{{%notice tip%}}
Trong quá trình làm các bước trong workshop này, hãy dùng IAM User có quyền **Administrator Access** thay vì sử dụng root account. \
Workshop này sẽ được thực hiện tại Region Singapore.
{{%/notice%}}

1. Truy cập vào [giao diện dịch vụ Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9/home/product)
 + Click **Create environment**.

2. Điền tên **StepFunctions**, sau đó click **Next step**.

3. Giữ nguyên tùy chọn cơ bản, kéo màn hình xuống dưới cùng, click **Next step**.

![StepFunctions](/images/SF/003.png?width==90pc)

4. Click **Create environment**, chờ vài phút để môi trường Cloud9 instance được khởi tạo hoàn tất.

5. Click biểu tượng **X** để đóng bớt các tab không cần thiết.
  + Click biểu tượng **+**, sau đó click chọn **New Terminal** để mở giao diện dòng lệnh mới.

![StepFunctions](/images/SF/004.png?width==90pc)

Bước tiếp theo chúng ta sẽ tạo các dịch vụ mẫu cho workflow của chúng ta.
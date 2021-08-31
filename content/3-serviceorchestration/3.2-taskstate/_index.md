+++
title = "Kiểu Task State"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2 </b>"
+++


#### Kiểu Task State

**Task state** khiến trình thông dịch thực thi công việc được xác định bởi trường **Resource** của trạng thái. Dưới đây, chúng ta sẽ sử dụng các trạng thái (state) **Task** để gọi  Lambda function của dịch vụ **Data Checking** , chuyển các thuộc tính tên và địa chỉ từ đơn đăng ký  vào các lệnh gọi Lambda function.

Trong các trạng thái Task bên dưới, ngoài việc chỉ định ARN của Lambda function mà chúng ta muốn trạng thái Task sử dụng, chúng ta cũng bao gồm phần Tham số. Đây là cách chuyển dữ liệu từ đầu vào của trạng thái vào đầu vào của Lambda function mục tiêu. Bạn sẽ nhận thấy rằng mình đang sử dụng cú pháp bao gồm các ký hiệu đô la ở cuối mỗi tên thuộc tính và đầu mỗi giá trị thuộc tính. Điều này là để Step Functions biết chúng ta muốn đưa dữ liệu trạng thái vào các tham số này thay vì thực sự gửi một chuỗi thô của $ .application.name chẳng hạn.

State machine sử dụng dưới đây giả định sẽ nhận một đối tượng đầu vào ban đầu với một thuộc tính duy nhất được gọi là **application** ( đại diện cho đăng ký ) có thuộc tính name ( tên ) và address ( địa chỉ ). Chúng ta sẽ chỉ định đầu vào này để thực thi trong giây lát sau khi chúng ta cập nhật định nghĩa state machine của mình.

#### Nội dung
1. [Cập nhật workflow](3.2.1-updateworkflow/)
2. [Kiểm tra workflow](3.2.2-tryworkflow/)

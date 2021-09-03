+++
title = "Kích hoạt workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>4.1 </b>"
+++


#### Kích hoạt workflow

Ở trạng thái **Pending Review**, chúng ta muốn state machine gọi đến dịch vụ **Account Applications** để gắn cờ các đăng ký cần kiểm tra lại, sau đó tạm dừng và đợi cuộc gọi lại từ dịch vụ **Account Applications**, điều này sẽ xảy ra sau khi kiểm soát viên kiểm tra và đưa ra quyết định.

Để Step Functions của chúng ta thông báo cho dịch vụ **Account Applications** rằng một đăng ký sẽ được gắn cờ, bạn cần phải chuyển cho nó một ID đăng ký. Và cách để Step Functions có thể chuyển một ID trở lại dịch vụ **Account Applications** là **thực hiện thêm vào thuộc tính ID như một phần của thông tin đăng ký khi bắt đầu thực thi workflow.**

Để thực hiện việc này, chúng ta sẽ tích hợp dịch vụ **Account Applications** với Step Functions xử lý đơn đăng ký, bắt đầu một quá trình thực thi mới mỗi khi đơn đăng ký mới được gửi đến dịch vụ. Khi chúng ta bắt đầu thực thi, ngoài việc chuyển tên và địa chỉ của người dùng làm đầu vào (để kiểm tra tên và địa chỉ ), chúng ta cũng sẽ chuyển ID đăng ký để Step Functions có thể thực thi chức năng **FlagApplication** của dịch vụ **Account Applications** nhằm gắn cờ các đơn đăng ký cần xem xét.

Trong bước này chúng ta sẽ :
  + Chuyển ARN của state machine làm biến môi trường cho hàm lambda **SubmitApplication**.

  + Cập nhật Lambda function **SubmitApplication**  để thực thi  Step Functions state machine của chúng ta khi đơn đăng ký mới được gửi, chuyển thông tin chi tiết về người đăng ký vào đầu vào của state machine.

  + Cấp quyền cho  Lambda function **SubmitApplication**  để thực thi state machine của chúng ta.

#### Nội dung
1. [Cập nhật state machine](4.1.1-updateworkflow/)
2. [Kiểm tra workflow](4.1.2-checkworkflow/)
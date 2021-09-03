+++
title = "Xử lý lỗi"
date = 2021
weight = 5
chapter = false
pre = "<b>5. </b>"
+++


#### Xử lý lỗi trong quá trình thực thi workflow bằng cách thêm khả năng retry và error handling (catch)

Cho tới thời điểm này workflow của chúng ta chưa có khả năng xử lý lỗi nào. Điều gì sẽ xảy ra nếu một số lệnh gọi Lambda function bị timeout hoặc nếu chúng gặp phải một số loại lỗi tạm thời khác? Điều gì sẽ xảy ra nếu các Lambda function xuất hiện các exception ? Hoặc  một trong các Lambda function đang gọi một dịch vụ của bên thứ ba. Cuộc gọi bên ngoài đó cũng có thể không thành công hoặc timeout. Chúng ta sẽ cùng nhau giải quyết những điều-nếu-xảy ra này và tận dụng khả năng thử lại ( retry ) và xử lý lỗi ( error handling - catch) được tích hợp sẵn của AWS Step Functions.

Vậy những loại lỗi có thể xảy ra? Dưới đây là một số lỗi tham khảo:

+ Bất kỳ trạng thái nào cũng có thể gặp lỗi thời gian chạy. Lỗi có thể xảy ra vì nhiều lý do:
  + Các vấn đề về định nghĩa state machine (ví dụ: không có quy tắc phù hợp nào trong trạng thái Lựa chọn)
  + Lỗi tác vụ (ví dụ:  Lambda function exception)
  + Sự cố tạm thời (ví dụ: sự cố về network)

Theo mặc định, khi một trạng thái báo lỗi, AWS Step Functions khiến việc thực thi hoàn toàn không thành công.

Đối với quy trình làm việc mẫu của chúng ta, chúng ta có thể đồng ý với việc cho phép quy trình làm việc của mình không thành công khi có bất kỳ lỗi không mong muốn nào xảy ra. Nhưng một số lỗi gọi Lambda function là tạm thời, vì vậy ít nhất chúng ta nên thêm một số hành vi thử lại vào trạng thái Task gọi các Lambda function.

#### Nội dung
1. [Thêm tính năng Retry](5.1-retry/)
2. [Thêm tính năng Catch](5.2-catch/)

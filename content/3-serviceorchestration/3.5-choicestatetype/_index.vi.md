+++
title = "Kiểu Choice State"
date = 2021
weight = 5
chapter = false
pre = "<b>3.5 </b>"
+++


#### Kiểu Choice State

**Choice state** thêm logic phân nhánh vào state machine. Bạn có thể coi điều này giống như một câu lệnh switch phổ biến trong nhiều ngôn ngữ lập trình. **Choice state** có một loạt các quy tắc ( **rules** ). Mỗi quy tắc chứa hai điều: một biểu thức đánh giá một số biểu thức boolean và một tham chiếu đến trạng thái tiếp theo để chuyển sang nếu quy tắc này khớp thành công. 

Tất cả các quy tắc được đánh giá theo thứ tự và quy tắc đầu tiên khớp thành công sẽ khiến state machine chuyển sang trạng thái tiếp theo được xác định bởi quy tắc.

Trong workflow làm việc mẫu của chúng ta, chúng ta muốn đợi một kiểm soát viên kiểm tra đơn đăng ký nếu hai trạng thái **Check Name** và **Check Address** trả về kết quả gắn cờ. Nếu không, chúng ta muốn tự động phê duyệt ứng dụng. Hãy thêm vào **Choice State** để triển khai workflow này.

Dưới đây là workflow cập nhật của chúng ta sẽ trông như thế nào sau khi chúng ta thực hiện xong bước này:

![StepFunctions](/images/SF/a01.png?width=50pc)

#### Nội dung
1. [Cập nhật state machine](3.5.1-updateworkflow/)
2. [Kiểm tra workflow](3.5.2-checkworkflow/)
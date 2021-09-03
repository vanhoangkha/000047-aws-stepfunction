+++
title = "Xử lý state song song"
date = 2021
weight = 6
chapter = false
pre = "<b>6. </b>"
+++


#### Xử lý state song song ( Parallel state )

Hiện tại, chúng ta đã thực hiện cả hai bước kiểm tra dữ liệu của mình lần lượt. Nhưng việc kiểm tra địa chỉ không phụ thuộc vào kết quả từ việc kiểm tra tên của người đăng ký. Vì vậy, đây là cơ hội tuyệt vời để tăng tốc mọi thứ và thực hiện song song hai bước kiểm tra dữ liệu đồng thời.

Step Functions cho phép chúng ta tạo trạng thái song song ( Parallel state ). Khi đó state machine thực hiện song song nhiều trạng thái. Trạng thái song song khiến trình thông dịch thực thi nhiều nhánh đồng thời bắt đầu với trạng thái có tên trong trường **StartAt** và đợi cho đến khi tất cả nhánh kết thúc (đạt đến trạng thái hoàn tất) trước khi xử lý trường **Next**  của trạng thái song song.

Trong bước này chúng ta sẽ:

+ Cập nhật state machine để chạy **Check Name** và **Check Address** song song sử dụng kiểu **Parallel** state.

+ Cập nhật state machine **Review Required?** để xử lý kết quả từ bước kiểm tra song song. Chúng ta cần làm bước này vì **Parallel** state sẽ trả kết quả của từng nhánh riêng lẻ dưới dạng element trong 1 array theo thứ tự được định nghĩa bởi **Parallel** state.

#### Nội dung
1. [Cập nhật workflow](6.1-updateworkflow/)
2. [Kiểm tra workflow](6.2-checkworkflow/)

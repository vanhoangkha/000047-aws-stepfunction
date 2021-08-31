+++
title = "State Input/Output"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++


#### State Input/Output

Mỗi lần thực thi state machine nhận một tài liệu JSON làm đầu vào và chuyển đầu vào đó đến trạng thái đầu tiên trong quy trình làm việc. Các trạng thái riêng lẻ nhận JSON làm đầu vào và chuyển JSON làm đầu ra cho trạng thái tiếp theo. Hiểu cách thông tin này chuyển từ trạng thái này sang trạng thái khác và học cách lọc và thao tác với dữ liệu này, là chìa khóa để thiết kế và triển khai hiệu quả quy trình công việc trong Step Functions của AWS. 

Bạn có thể tham khảo [Phần Xử lý đầu vào và đầu ra của hướng dẫn dành cho nhà phát triển](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-input-output-filtering.html) để có thông tin chi tiết, hiện tại chúng ta sẽ chỉ đề cập đến kiến ​​thức cần thiết để state machine của chúng ta hoạt động.

Đầu ra của một trạng thái có thể là bản sao của đầu vào hoặc kết quả mà nó tạo ra (ví dụ: đầu ra từ Lambda Functioncủa trạng thái **Task** ) hoặc kết hợp giữa đầu vào và kết quả của nó. Chúng ta có thể sử dụng thuộc tính **ResultPath** bên trong các định nghĩa tác vụ state machine của mình để kiểm soát tổ hợp nào của các cấu hình kết quả này được chuyển tới đầu ra trạng thái (sau đó trở thành đầu vào cho trạng thái tiếp theo).

{{%notice tip%}}
Lý do khiến việc thực thi không thành công ở bước 3.3 là do **hành vi mặc định của một Task, nếu không chỉ định thuộc tính ResultPath, là lấy đầu ra của tác vụ và sử dụng nó làm đầu vào cho trạng thái tiếp theo**. Trong trường hợp của chúng ta, vì trạng thái trước đó (Check Name) đã tạo đầu ra là {"flagged": false} nên nó đã trở thành đầu vào cho trạng thái tiếp theo (Check Address). \
\
Điều ta muốn làm là giữ nguyên thông tin đầu vào ban đầu, chứa thông tin của người đăng ký, hợp nhất kết quả của **Check Name** vào trạng thái đó và chuyển toàn bộ nội dung xuống **Check Address**. Sau đó, **Check Address** có thể làm tương tự. Những gì chúng ta muốn làm là làm cho cả hai bước kiểm tra dữ liệu thực thi một cách chính xác và hợp nhất các đầu ra của chúng với nhau.

{{%/notice%}}

 
Vì vậy, để khắc phục sự cố hiện tại, chúng tôi cần thêm câu lệnh ResultPath, hướng dẫn state machine tạo đầu ra của nó bằng cách lấy đầu ra của Lambda function và hợp nhất nó với đầu vào của trạng thái. Đó là một thay đổi đơn giản. Chúng tôi chỉ cần thêm một chút cấu hình bổ sung vào định nghĩa của Task state: **"ResultPath": "$.SomePropertyName"**. 

Trong Amazon States Language, cú pháp ký hiệu đô la mà bạn thấy ở đây có nghĩa là đầu vào của state. Chúng ta sẽ đặt kết quả của việc thực thi tác vụ này (trong trường hợp này là đầu ra của Lambda function) vào một thuộc tính mới của đối tượng chứa trạng thái đầu vào và sử dụng nó làm đầu ra của trạng thái.
{{%notice info%}}
 Ví dụ : **"ResultPath": "$.checks.name"** , nghĩa là đặt kết quả của state **Check Name** vào một thuộc tính mới có tên **checks.name** của đối tượng chứa trạng thái đầu vào.
{{%/notice%}}

#### Nội dung
1. [Cập nhật state machine](3.4.1-updateworkflow/)
2. [Kiểm tra workflow](3.4.2-checkworkflow/)
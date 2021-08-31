+++
title = "Cập nhật Permissions"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++


#### Cập nhật Permissions

Thay vì tiếp tục làm việc trong bảng điều khiển web và thực hiện sửa lỗi  bằng tay, chúng ta sẽ sử dụng công cụ AWS SAM để xác định state machine cùng với các tài nguyên khác được sử dụng trong workshop này và thiết lập quyền để state machine này thực thi thành công.

Các bước chính chúng ta sẽ làm :
  + Khai báo state machine của chúng ta bên trong một file mới  tại statemachine/ account-application-workflow.asl.json

  + Cập nhật file SAM template của chúng ta template.yaml để bao gồm tài nguyên  triển khai state machine mới.

  + Thêm một IAM role mới cho state machine  để đảm nhận khi nó thực thi. Vai trò cấp quyền cho state machine gọi Lambda function **Data Checking**.

{{%notice tip%}}
Trước khi chúng ta di chuyển định nghĩa state machine sang file template.yaml, chúng  ta xóa state machine trong giao diện console của Step Functions để không bị nhầm lẫn khi một state machine tương tự được triển khai.
{{%/notice%}}

#### Nội dung
1. [Xóa state machine](3.3.1-deleteoldworkflow/)
2. [Cập nhật SAM template](3.3.2-updatesamtemplate/)
3. [Kiểm tra workflow](3.3.3-checkworkflow/)
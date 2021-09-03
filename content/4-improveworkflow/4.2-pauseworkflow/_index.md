+++
title = "Tạm dừng workflow để chờ kiểm tra hoàn tất"
date = 2021
weight = 2
chapter = false
pre = "<b>4.2 </b>"
+++


#### Tạm dừng workflow để chờ kiểm tra hoàn tất

**Step Functions** thực hiện công việc của mình bằng cách tích hợp trực tiếp với các dịch vụ AWS khác nhau và bạn có thể kiểm soát các dịch vụ AWS này bằng cách sử dụng ba cách tích hợp dịch vụ dưới đây:

  + Gọi một dịch vụ và để cho Step Functions chuyển sang trạng thái tiếp theo ngay sau khi nó nhận được  HTTP response. Bạn đã thấy loại tích hợp này hoạt động thông qua việc chúng ta sử dụng để gọi Lambda function kiểm tra dữ liệu và nhận lại phản hồi.

  + Gọi một dịch vụ và để Step Functions chờ công việc hoàn tất. Điều này thường được sử dụng  để kích hoạt khối lượng công việc theo kiểu hàng loạt ( batch ), tạm dừng, sau đó tiếp tục thực thi sau khi công việc hoàn thành. Chúng ta không sử dụng cách tích hợp này trong workshop hiện tại.

  + Gọi một dịch vụ có task token và để Step Functions chờ cho đến khi mã thông báo đó được phản hồi cùng với payload ( nội dung ). Đây là cách tích hợp mà chúng ta muốn sử dụng ở đây, vì chúng ta muốn thực hiện gọi dịch vụ, sau đó chờ phản hồi rồi mới tiếp tục thực thi.

{{%notice info%}}

Phản hồi ( call back ) cung cấp một cách để tạm dừng một quy trình công việc cho đến khi một task token được chuyển lại. Một nhiệm vụ có thể cần phải đợi sự chấp thuận của kiểm soát viên, tích hợp với bên thứ ba hoặc gọi các hệ thống kế thừa. Đối với những tác vụ như thế này, bạn có thể tạm dừng việc thực thi Step Functions state machine và đợi một quy trình hoặc quy trình công việc bên ngoài hoàn thành.
{{%/notice%}}

Bạn có thể điều chỉnh trạng thái Task tạo task token duy nhất ( task token ) (một ID duy nhất tham chiếu trạng thái Task cụ thể trong một lần thực thi cụ thể),  gọi dịch vụ AWS mong muốn , sau đó tạm dừng thực thi cho đến khi dịch vụ Step Functions nhận được task token vụ trở lại thông qua lệnh gọi API từ một số quy trình khác.

Trong bước này chúng ta sẽ :

  + Đặt trạng thái **Pending Review** của chúng ta gọi Lambda function **Account Applications** bằng cách sử dụng cú pháp định nghĩa trạng thái Tác vụ bao gồm hậu tố .waitForTaskToken. Thao tác này sẽ tạo task token mà chúng ta có thể chuyển cho dịch vụ **Account Applications** cùng với ID đăng ký mà chúng ta muốn gắn cờ.

  + Làm cho các **Account Applications** Lambda function cập nhật hồ sơ đăng ký để đánh dấu nó là **Pending Review** và lưu trữ task token cùng với hồ sơ. Sau đó, khi kiểm soát viên xem xét đăng ký đang chờ xử lý, dịch vụ **Account Applications** sẽ thực hiện gọi lại API Step Functions, gọi điểm cuối **SendTaskSuccesss**, chuyển lại task token cùng với đầu ra có liên quan từ bước này, trong trường hợp của chúng ta, sẽ là dữ liệu để cho biết liệu kiểm soát viên đã chấp thuận hay từ chối đăng ký. Thông tin này sẽ cho phép state machine quyết định phải làm gì tiếp theo dựa trên quyết định của kiểm soát viên.

  + Tạo một Lambda function **ReviewApplication** mới (và các quyền IAM cần thiết cho nó) để triển khai logic cho phép kiểm soát viên đưa ra quyết định cho một đăng ký được gắn cờ, gọi lại API Step Functions với kết quả là quyết định của kiểm soát viên.

  + Thêm một trạng thái Lựa chọn khác có tên là **Review Approved?** sẽ kiểm tra kết quả từ trạng thái **Pending Review** và chuyển sang trạng thái **Approve Application** hoặc trạng thái **Reject Application** mà chúng ta cũng sẽ thêm .

#### Nội dung
1. [Cập nhật state machine](4.2.1-updateworkflow/)
2. [Kiểm tra workflow](4.2.2-checkworkflow/)
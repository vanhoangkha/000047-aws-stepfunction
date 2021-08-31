+++
title = "AWS Step Functions"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++


#### AWS Step Functions

AWS Step Functions cho phép bạn điều phối các dịch vụ thông qua quy trình làm việc được quản lý hoàn toàn để bạn có thể xây dựng và cập nhật ứng dụng một cách nhanh chóng. Quy trình làm việc được tạo thành từ một loạt các bước, với đầu ra của một bước đóng vai trò là đầu vào cho bước tiếp theo. 

Việc phát triển ứng dụng đơn giản và trực quan hơn bằng cách sử dụng Step Functions vì nó chuyển quy trình làm việc của bạn thành một sơ đồ **state machine** dễ hiểu, dễ giải thích cho người khác và dễ thay đổi. Bạn có thể theo dõi từng bước trong quá trình thực thi quy trình công việc khi chúng xảy ra (và cả sau đó), điều này giúp bạn nhanh chóng xác định các vấn đề và khắc phục chúng. Step Functions tự động kích hoạt và theo dõi từng bước, đồng thời có thể thử lại các bước khi có lỗi, để quy trình làm việc ứng dụng của bạn thực thi một cách đáng tin cậy và theo thứ tự bạn mong đợi.

Có rất nhiều điều để khám phá trong phần mô tả ở trên và vào cuối workshop này, bạn sẽ thấy tất cả những lợi ích đó. Nhưng cách tốt nhất để hiểu một dịch vụ mới là sử dụng nó, vì vậy chúng ta sẽ đi sâu vào Step Functions bằng cách bắt đầu với các khái niệm và bước cơ bản mà chúng ta cần để kết nối các dịch vụ của mình với nhau.

Step Functions hoạt động bằng cách biểu diễn quy trình làm việc của chúng ta dưới dạng **state machine**. Khái niệm **state machine** chỉ là sự hình thức hóa những thứ mà bạn  đã có hiểu biết trực quan rất rõ ràng từ việc viết bất kỳ mã lập trình cơ bản nào.

**State machine**  là một cách  mô tả một tập hợp các bước quy trình làm việc được chia thành các trạng thái được đặt tên. Mỗi state machine có một trạng thái khởi động và luôn chỉ có một trạng thái hoạt động (trong quá trình thực thi). Trạng thái hoạt động có một số đầu vào và thường thực hiện một số hành động bằng cách sử dụng đầu vào đó, điều này tạo ra kết quả đầu ra. **State machine** chuyển đổi từ trạng thái này sang trạng thái tiếp theo dựa trên trạng thái của chúng và các kết nối rõ ràng mà chúng ta cho phép giữa các trạng thái.

Hãy bắt tay thực hành để bạn có thể thấy cách hoạt động của **Step Functions**. Chúng ta sẽ bắt đầu bằng cách viết Step Functions state machine để tạo mô hình  đơn giản hóa của quy trình làm việc đăng ký tài khoản. Với AWS Step Functions, có một số loại trạng thái (hoặc các bước) khác nhau mà chúng ta có thể sử dụng để tạo state machine  quy trình làm việc của mình và đơn giản nhất là Trạng thái **Pass**. Trạng thái Pass chỉ đơn giản là chuyển đầu vào của nó đến đầu ra của nó, không thực hiện công việc nào. Các trạng thái vượt qua rất hữu ích khi xây dựng và gỡ lỗi cho state machine, vì vậy chúng ta sẽ sử dụng chúng ở bước này để bắt đầu phác thảo quy trình làm việc của mình.

#### Nội dung
1. [Khởi tạo workflow](3.1.1-workflow/)
2. [Kiểm tra workflow](3.1.2-tryworkflow/)

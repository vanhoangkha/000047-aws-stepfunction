+++
title = "Dịch vụ điều phối"
date = 2021
weight = 3
chapter = false
pre = "<b>3. </b>"
+++


#### Dịch vụ điều phối

Trở lại với câu hỏi ở phần trước của chúng ta là chúng ta điều phối các dịch vụ vào quy trình bằng cách nào.

Thoạt nhìn, giải pháp đơn giản nhất với bạn là  thêm logic vào dịch vụ **Account Application**. Khi đơn đăng ký được gửi, chúng ta có thể thực hiện hai cuộc gọi liên dịch vụ tới dịch vụ **Data Checking** để kiểm tra tên và địa chỉ. Sau đó, khi  có câu trả lời cho cả hai, chúng ta có thể gắn cờ cho đăng ký để xem xét nếu một trong hai lần **Data Checking** có gắn cờ, nếu không, chúng ta có thể phê duyệt đăng ký tự động. Chắc chắn cách tiếp cận này sẽ hiệu quả, nhưng có một số nhược điểm sẽ xuất hiện sau đó.

Đầu tiên, chúng ta sẽ cần xử lý các điều kiện thời gian chờ và lỗi một cách tinh tế. Điều gì sẽ xảy ra nếu chúng tôi không nhận được phản hồi kịp thời từ cuộc gọi liên dịch vụ của mình hoặc nếu ta gặp lỗi tạm thời và muốn thử lại yêu cầu? Nếu chúng ta đang liên kết các dịch vụ này lại với nhau trực tiếp trong logic của dịch vụ, chúng ta sẽ cần thêm một số logic **dự phòng / thử lại**. Về bản chất, đây không phải là vấn đề lớn, nhưng nó làm chậm chuỗi xử lý chính của chúng ta trong khi nó đang chờ thử lại các yêu cầu đó.

Một lợi ích khác bị bỏ lỡ ở đây là nếu chúng ta chỉ triển khai logic quy trình trong bản thân dịch vụ, thì sẽ không dễ dàng để tạo bản trình bày trực quan về thiết kế quy trình làm việc này hoặc lịch sử thực thi của nó. Việc hình dung ra hình dạng của quy trình kinh doanh mang lại giá trị to lớn, vì nó giúp người dùng không chuyên về kỹ thuật hiểu và xác thực logic nghiệp vụ mà chúng ta đang sử dụng trong hệ thống của mình. Hơn nữa, quy trình công việc này bắt đầu rất đơn giản, nhưng khi quy trình công việc của chúng ta tăng độ phức tạp lên, việc kiểm tra toàn bộ quá trình thực thi quy trình công việc  ngày càng trở nên khó khăn hơn và việc gỡ lỗi cũng không dễ dàng.

AWS có một công cụ đơn giản nhưng cực kỳ mạnh mẽ để giúp chúng ta sắp xếp quy trình công việc của mình theo cách giải quyết tất cả những mối quan tâm này: **AWS Step Functions**. 

#### Nội dung
1. [AWS Step Functions](3.1-stepfunctions/)
2. [Kiểu Task State](3.2-taskstate/)
3. [Cập nhật Permissions](3.3-updatepermission/)
4. [State Input/Output](3.4-stateinputoutput/)
5. [Kiểu Choice State](3.5-choicestatetype/)

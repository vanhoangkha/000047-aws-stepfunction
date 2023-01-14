+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.1.2 </b>"
+++


#### Kiểm tra workflow

Tại thời điểm này, mặc dù chúng ta đã tạo một state machine hợp lệ, nhưng nó không thực sự làm được gì vì trạng thái Pass mà chúng tôi đang sử dụng trong định nghĩa của chúng tôi chỉ chuyển đầu vào thẳng tới đầu ra của nó mà không thực hiện bất kỳ công việc nào. State machine của chúng ta chỉ chuyển đổi qua ba trạng thái Pass và kết thúc. Tuy nhiên, hãy nhanh chóng thử thực thi  để chúng ta có thể thấy điều này.



1. Click **Start execution**.

![AWS Step Functions](/images/3.1.2/0001.png?featherlight=false&width=90pc)

2. Mỗi khi chúng ta yêu cầu Step Functions thực thi một state machine, chúng ta có thể cung cấp một số đầu vào ban đầu. Chúng ta hãy giữ nguyên đầu vào ví dụ ban đầu và click **Start execution**.


![AWS Step Functions](/images/3.1.2/0002.png?featherlight=false&width=90pc)

3. Bây giờ bạn sẽ thấy trang chi tiết về quá trình thực thi mà chúng ta vừa kích hoạt. Nhấp vào bất kỳ tên bước nào trong hình ảnh trực quan và lưu ý cách chúng ta có thể xem các giá trị đầu vào và đầu ra cho mỗi trạng thái trong quá trình thực thi.

![AWS Step Functions](/images/3.1.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.2/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.2/0006.png?featherlight=false&width=90pc)

Bây giờ bạn đã hiểu cách xác định và thực thi state machine, hãy cập nhật định nghĩa state machine của chúng ta để thực sự thực hiện một số công việc bằng cách gọi đến dịch vụ **Data Checking** để kiểm tra tên và địa chỉ. Đối với định nghĩa mới của chúng ta, chúng ta sẽ sử dụng trạng thái **Task** để thực hiện công việc.
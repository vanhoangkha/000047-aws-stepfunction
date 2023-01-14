+++
title = "Kiểm tra workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.4.2 </b>"
+++


#### Kiểm tra workflow


1. Truy cập vào [giao diện State machines](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click vào state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
 
![AWS Step Functions](/images/3.4.2/0001.png?featherlight=false&width=90pc)

2. Click vào lần thực thi cuối cùng của state machine. ( đang bị failed )
 + Click **New execution** để thực thi một lần mới.

![AWS Step Functions](/images/3.4.2/0002.png?featherlight=false&width=90pc)

3. Để phần input như cũ và click **Start execution**.

![AWS Step Functions](/images/3.4.2/0003.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0004.png?featherlight=false&width=90pc)

4. Chúng ta sẽ thấy workflow được thực thi thành công.
  + Click state **Check Address**.
  + Click **Step Input**.

![AWS Step Functions](/images/3.4.2/0005.png?featherlight=false&width=90pc)

5. Click **Step Output** để kiểm tra kết quả đầu ra.

![AWS Step Functions](/images/3.4.2/0006.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0008.png?featherlight=false&width=90pc)


{{%notice tip%}}
Lưu ý cách trạng thái **Check Name** giữ đầu vào ban đầu của chúng ta và thêm kết quả của nó vào bên trong **$ .checks.name** và cách **Check Address** lấy đầu ra đó làm đầu vào và thêm kết quả kiểm tra địa chỉ của chính nó vào bên trong **$ .checks.address**. Đó là sức mạnh của ResultPath.
{{%/notice%}}

Tại thời điểm này, chúng ta có một workflow thực thi thành công, nhưng workflow này vẫn thiếu một số logic quan trọng. workflow làm việc của chúng ta đi trực tiếp từ **Check Address** đến **Approve Application**. Những gì chúng ta muốn là chỉ tự động phê duyệt một ứng dụng nếu cả tên và địa chỉ không bị gắn cờ, và nếu đơn bị gắn cờ chúng ta sẽ xếp đơn đăng ký để kiểm soát viên thực hiện kiểm tra lại.

Chúng ta sẽ kết hợp một bước đợi phản hồi của kiểm soát viên trên các đơn đăng ký bị gắn cờ. Nhưng trước đó, chúng ta cần học cách kiểm tra trạng thái của workflow làm việc và thực thi một số logic phân nhánh dựa trên các kiểm tra mà chúng ta xác định. Để thực hiện việc này, chúng tôi sẽ cần thêm một loại trạng thái mới vào state machine của mình được gọi là trạng thái Lựa chọn ( Choice state ).
+++
title = "Chi tiết workflow"
date = 2021
weight = 3
chapter = false
pre = "<b>1.3 </b>"
+++


#### Chi tiết workflow

Bất cứ khi nào có đăng ký tài khoản mới vào hệ thống mẫu của chúng ta, chúng ta sẽ thực hiện kiểm tra một số thông tin của người đăng ký và sau đó tự động phê duyệt đơn đăng ký nếu không có vấn đề gì với dữ liệu hoặc gắn cờ nó để kiểm soát viên thực hiện xem xét. Kiểm soát viên sẽ định kỳ kiểm tra các đơn đăng ký bị gắn cờ và quyết định phê duyệt hoặc từ chối đơn đó. 

Tóm lại, đây là quy trình công việc mà chúng ta sẽ quản lý:

1. Kiểm tra tên và địa chỉ của người đăng ký đối với một dịch vụ để phát hiện xem họ có đáng ngờ hay không hoặc cần được kiểm soát viên trước khi xử lý đơn đăng ký tài khoản.

2. Nếu tên và địa chỉ được kiểm tra mà không có bất kỳ vấn đề gì sẽ tự động phê duyệt đơn đăng ký. Nếu gặp lỗi khi cố gắng kiểm tra tên hoặc địa chỉ, hãy gắn cờ đơn đăng ký là không thể xử lý và dừng lại. Ngược lại, nếu việc kiểm tra tên hoặc địa chỉ cho thấy có điều bất thường về dữ liệu, hãy tiếp tục bước tiếp theo.

3. Gắn cờ đơn đăng ký để kiểm soát viên xử lý và tạm dừng xử lý thêm.

4. Chờ một kiểm soát viên kiểm tra đơn đăng ký được gắn cờ và đưa ra quyết định phê duyệt hoặc từ chối.

5. Phê duyệt hoặc từ chối đơn đăng ký, tùy theo quyết định của Kiểm soát viên.

Dưới đây là một sơ đồ để minh họa quy trình làm việc của chúng ta:

![StepFunctions](/images/SF/002.png?width=60pc)

+++
title = "Service Orchestration"
date = 2021
weight = 1
chapter = false
pre = "<b>1.1 </b>"
+++


#### Service Orchestration ( Dịch vụ điều phối )

Chúng ta sử dụng thuật ngữ Orchestration để mô tả hành động điều phối các dịch vụ phân tán thông qua việc quản lý quy trình làm việc tập trung, tương tự như cách nhạc trưởng hiểu từng bộ phận của dàn nhạc và chỉ đạo từng bộ phận nhạc cụ hoạt động cùng nhau để tạo ra màn trình diễn của mình.

Lợi ích chính của Orchestration đối với việc điều phối dịch vụ là nó quản lý logic tập trung, thay vì quản lý logic phân tán ở mức dịch vụ ( bản thân mỗi dịch vụ tự quyết định mình cần làm gì tiếp theo ). Chúng ta có thể hiểu toàn bộ quy trình từ đầu đến cuối thông qua một giao diện làm việc duy nhất.

Cách tiếp cận này hoạt động tốt khi các dịch vụ có mức độ phối hợp cao, cần thực hiện chạy thử lại , xử lý lỗi cụ thể , chạy tác vụ song song hoặc đợi một số tác vụ hoàn thành trước khi tiếp tục thực hiện các tác vụ tiếp theo trong workflow ( quy trình làm việc ).

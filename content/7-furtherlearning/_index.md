+++
title = "Tìm hiểu thêm"
date = 2021
weight = 7
chapter = false
pre = "<b>7. </b>"
+++


#### Tìm hiểu thêm về AWS Step Functions

Step Functions có thể làm nhiều việc hơn là điều phối các Lambda functions.

+ [**Activities**](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html) là 1 tính năng của AWS Step Functions cho phép bạn tạo 1 task trong state machine mà phần công việc phía sau được thực hiện bởi tài nguyên tính toán ở bất kì đâu bao gồm Amazon Elastic Compute Cloud ( Amazon EC2 ), Amazon Elastic Container Service ( Amazon ECS ), các thiết bị mobile, hoặc bất kỳ đâu mà chúng ta có thể kết nối tới AWS Step Functions API thông qua HTTPS.

+ [**Map**](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-map-state.html) state có thể được sử dụng để chạy một tập các bước cho mỗi element trong input array. Trong khi **Parallel** state thực thi nhiều nhánh sử dụng chung 1 input, **Map** state sẽ thực thi một tập các bước nhiều lần với mỗi element trong input array.

+ [**Wait**](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-wait-state.html) state trì hoãn state machine tiếp tục thực thi trong 1 thời gian chỉ định. Bạn có thể cấu hình thời gian liên quan tới các state , ví dụ thời gian tính từ lúc state bắt đầu thực thi. Ngoài ra bạn cũng có thể chỉnh thời gian cố định.

Step Functions có thể tích hợp trực tiếp với rất nhiều dịch vụ AWS bao gồm AWS Batch, Amazon DynamoDB, Amazon ECS và Fargate, Amazon SQS và SNS, AWS Glue, Amazon Sage Maker... Hãy tham khảo [danh sách đầy đủ](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-service-integrations.html) nhé.


{{%notice tip%}}
Nếu bạn muốn tìm hiểu sâu hơn nữa về Step Functions, bước tốt nhất là thông qua [**Developer Guide**](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html). Sau đó là tìm hiểu các [**project mẫu**](https://docs.aws.amazon.com/step-functions/latest/dg/create-sample-projects.html). Cuối cùng là các webinars, blog , kiến trúc tham khảo, videos và các tài nguyên tại trang [**AWS Step Functions Resources**](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-service-integrations.html).
{{%/notice%}}

+++
title = "Bắt đầu với AWS Step Functions"
date = 2021
weight = 1
chapter = false
+++

# Bắt đầu với AWS Step Functions

#### Tổng quan

Trong workshop này, bạn sẽ học cách điều phối quy trình công việc giữa các dịch vụ  bằng cách sử dụng một dịch vụ đơn giản nhưng mạnh mẽ, được quản lý hoàn toàn bởi AWS, AWS Step Functions. Chúng ta sẽ triển khai một cặp dịch vụ dưới dạng các AWS Lambda functions đơn giản và sắp xếp chúng lại với nhau thành một quy trình công việc ví dụ liên quan đến việc thực thi song song, logic phân nhánh và tạm dừng / tiếp tục các công việc.

#### Mục tiêu

Thông qua workshop chúng ta sẽ nắm được:

+ Lợi ích của việc triển khai tổ chức dịch vụ bằng việc sử dụng dịch vụ AWS Step Functions.
+ Cơ bản về việc sử dụng AWS Step Functions state machines:
  + Làm việc với Lambda Functions sử dụng **Task** state.
  + Thực hiện logic phân nhánh sử dụng **Choice** state.
  + Thực hiện công việc song song sử dụng **Parallel** state.
  + Thực hiện **Pause/Resume** dựa trên token & callback sử dụng **waitForTaskToken**.
+ Visualizing, debugging and auditing workflow sử dụng AWS Step Functions console.

#### Phiên bản tiếng Anh
[Intro to Service Coordination using AWS Step Functions](https://step-functions-workshop.go-aws.com/)

#### Nội dung
1. [Giới thiệu workshop](1-intro/)
2. [Chuẩn bị](2-prepare/)
3. [Dịch vụ điều phối ](3-serviceorchestration/)
4. [Cải tiến workflow](4-improveworkflow/)
5. [Xử lý lỗi](5-errorhandling/)
6. [Xử lý state song song](6-stateparallel/)
7. [Tìm hiểu thêm](7-furtherlearning/)
8. [Dọn dẹp tài nguyên](8-cleanup/)
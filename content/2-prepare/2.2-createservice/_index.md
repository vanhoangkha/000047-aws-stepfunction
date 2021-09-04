+++
title = "Tạo các dịch vụ mẫu"
date = 2021
weight = 2
chapter = false
pre = "<b>2.2 </b>"
+++


#### Tạo các dịch vụ mẫu - Tổng quan

Trong workshop này, chúng tôi sẽ sử dụng  AWS Serverless Application Model (SAM) để viết, triển khai và kiểm tra chức năng trong các dịch vụ của mình. Bạn không cần phải có  kinh nghiệm nào với AWS SAM; workshop sẽ giải thích khi chúng ta tiếp tục, qua đó bạn cũng có thể tìm hiểu cách SAM giúp bạn xây dựng hệ thống serverless.

Trong bước này chúng ta sẽ tạo một số Lambda function, khi được gọi chung, có thể được coi là dịch vụ **Account Application** của chúng ta. Cụ thể, chúng ta sẽ thực hiện các chức năng cho phép  gửi các đăng ký mới (chỉ bao gồm một hồ sơ có tên và địa chỉ), gắn cờ các đăng ký để xem xét và đánh dấu các đăng ký là được chấp thuận hoặc bị từ chối. Chúng ta cũng sẽ bổ sung thêm khả năng liệt kê tất cả các đăng ký ở một trạng thái nhất định (như ĐÃ ĐƯỢC GỬI hoặc ĐÃ ĐƯỢC GẮN CỜ) để tránh rắc rối khi kiểm tra trực tiếp kho dữ liệu của dịch vụ. 

#### Cài đặt AWS SAM CLI
1. Trong giao diện terminal của Cloud9 instance, chạy lệnh dưới đây.
  + Đặt password sudo mới cho ec2-user của Cloud9 instance, ví dụ **123456**.

```
sudo passwd ec2-user
```

![StepFunctions](/images/SF/005.png?width==90pc)

2. Trong giao diện terminal của Cloud9 instance, chạy lệnh dưới đây.

```
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
  + Điền [sudo] password cho ec2-user : **123456**.
  + Ấn phím **Enter**.

![StepFunctions](/images/SF/006.png?width==90pc)

3. Kiểm tra quá trình cài đặt diễn ra thành công.

![StepFunctions](/images/SF/007.png?width==90pc)

4. Trong giao diện terminal của Cloud9 instance, chạy lệnh dưới đây để tiến hành cấu hình PATH cho homebrew và clone repo của workshop.
```
# Then get brew into our $PATH
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile

# Install the AWS SAM CLI
brew tap aws/tap
brew install aws-sam-cli

# Bootstrap our initial services into ./workshop-dir
git clone https://github.com/gabehollombe-aws/step-functions-workshop.git          
cp -R step-functions-workshop/sam_template/ workshop-dir

# Change to our new directory
cd workshop-dir
```
  + Quá trình cài đặt sẽ kéo dài vài phút.

![StepFunctions](/images/SF/008.png?width==90pc)

5. Chạy câu lệnh dưới đây để thực hiện build các Lambda functions, chuẩn bị cho việc triển khai.
```
sam build
```

![StepFunctions](/images/SF/009.png?width==90pc)

6. Chạy câu lệnh dưới đây để thực hiện deploy các Lambda functions và DynamoDB, lưu ý trả lời các thông tin như dưới đây :
  + Thông tin Stack Name [sam-app]: **step-functions-workshop**.
  + Thông tin AWS Region [ap-southeast-1]: **ap-southeast-1**.
  +  Confirm changes before deploy [y/N]: N
  +  Allow SAM CLI IAM role creation [Y/n]: Y
  +  Save arguments to configuration file [Y/n]: Y
  +  SAM configuration file [samconfig.toml]: samconfig.toml
  +  SAM configuration environment [default]: Ấn Enter

```
# Run our first deploy in guided mode. Answer the interactive questions the same way that the comments show below:
sam deploy --guided
```

![StepFunctions](/images/SF/010.png?width==90pc)

{{%notice warning%}}
Nếu bạn bị lỗi ở bước deploy hãy thử thay bước 6 bằng các bước ở mục [Chạy sam deploy bằng file config tự tạo.](#chạy-sam-deploy-bằng-file-config-tự-tạo)
{{%/notice%}}

7. Dành thời gian kiểm tra nội dung các files chúng ta đã tạo trong thư mục **workshop-dir** :

![StepFunctions](/images/SF/011.png?width==90pc)

```bash
functions/account-applications/submit.js
# Lambda SubmitApplication function: tìm, gắn cờ, từ chối và phê duyệt đăng ký. 

functions/account-applications/AccountApplications.js
# Đây là file chung cung cấp các tác vụ CRUD cho **Account Application** data type. Được sử dụng bởi các function khác.

functions/data-checking/data-checking.js
# File này triển khai **DataChecking** Lambda function, nhiệm vụ của nó là xử lý các yêu cầu kiểm tra tên và địa chỉ với một số rule đơn giản.

template.yaml
# Đây là file AWS SAM sẽ khai báo các tài nguyên mà chúng ta muốn tạo và triển khai trong giải pháp của mình, có cấu trúc khá tương tự CloudFormation template vì SAM được xây dựng trên nền tảng của CloudFormation.
```
Bước tiếp theo chúng ta sẽ kiểm tra các dịch vụ mẫu cho workflow của chúng ta.

#### Chạy sam deploy bằng file config tự tạo.

{{%notice warning%}}
Chỉ thực hiện bước này nếu việc thực hiện sam deploy -guided không thành công.
{{%/notice%}}

1. Kiểm tra đường dẫn thư mục đang ở workshop-dir
2. Chạy lệnh dưới đây để tạo SAM CLI bucket ( bạn có thể thay yourname = tên bạn )
```
aws s3 mb s3://stepfunctions-workshop-yourname --region ap-southeast-1
```
3. Chạy lệnh dưới đây để tạo file samconfig.toml

```
touch samconfig.toml
```
4. Double click vào file samconfig.toml và thực hiện thêm nội dung cấu hình như sau :
```
version=0.1
[default.global.parameters]
stack_name = "step-functions-workshop"

[default.deploy.parameters]
stack_name = "step-functions-workshop"
s3_bucket = "stepfunctions-workshop-yourname"
s3_prefix = "stepfunctions"
region = "ap-southeast-1"
confirm_changeset = false
capabilities = "CAPABILITY_IAM"
```
  + Ấn Ctrl+S để lưu file samconfig.toml.

![StepFunctions](/images/SF/f01.png?width==90pc)

5. Chạy lệnh dưới để thực hiện deploy lại.
```
sam deploy
```
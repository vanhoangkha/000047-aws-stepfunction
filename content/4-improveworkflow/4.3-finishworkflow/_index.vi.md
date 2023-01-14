+++
title = "Hoàn tất workflow"
date = 2021
weight = 3
chapter = false
pre = "<b>4.3 </b>"
+++


#### Hoàn tất workflow

Hiện tại, chúng ta vẫn để trống trạng thái **Approve Application** và **Reject Application**, ( sử dụng trạng thái **Pass**, không thực hiện tác vụ cụ thể ). Chúng ta sẽ thay thế các trạng thái này bằng các trạng thái Tast gọi các Lambda function thích hợp.

Trong bước này chúng ta sẽ :

+ Thay đổi trạng thái **Approve Application** và **Reject Application** từ trạng thái **Pass** sang trạng thái **Task** và gọi các Lambda function thích hợp. 
+ Cấp quyền bổ sung cho IAM Role mà Step Functions thực thi để nó có thể gọi các Lambda function cần thiết từ dịch vụ **Account Applications**.

1. Quay trở lại giao diện dòng lệnh của Cloud9 instance, thay thế nội dung của file **statemachine/account-application-workflow.asl.json** với nội dung dưới đây.
```
{
    "StartAt": "Check Name",
    "States": {
        "Check Name": {
            "Type": "Task",
            "Parameters": {
                "command": "CHECK_NAME",
                "data": {
                    "name.$": "$.application.name"
                }
            },
            "Resource": "${DataCheckingFunctionArn}",
            "ResultPath": "$.checks.name",
            "Next": "Check Address"
        },
        "Check Address": {
            "Type": "Task",
            "Parameters": {
                "command": "CHECK_ADDRESS",
                "data": {
                    "address.$": "$.application.address"
                }
            },
            "Resource": "${DataCheckingFunctionArn}",
            "ResultPath": "$.checks.address",
            "Next": "Review Required?"
        },
        "Review Required?": {
            "Type": "Choice",
            "Choices": [
                {
                    "Variable": "$.checks.name.flagged",
                    "BooleanEquals": true,
                    "Next": "Pending Review"
                },
                {
                    "Variable": "$.checks.address.flagged",
                    "BooleanEquals": true,
                    "Next": "Pending Review"
                }
            ],
            "Default": "Approve Application"
        },
        "Pending Review": {
            "Type": "Task",
            "Resource": "arn:aws:states:::lambda:invoke.waitForTaskToken",
            "Parameters": {
                "FunctionName": "${FlagApplicationFunctionName}",
                "Payload": {
                    "id.$": "$.application.id",
                    "flagType": "REVIEW",
                    "taskToken.$": "$$.Task.Token"
                }
            },
            "ResultPath": "$.review",
            "Next": "Review Approved?"
        },
        "Review Approved?": {
            "Type": "Choice",
            "Choices": [
                {
                    "Variable": "$.review.decision",
                    "StringEquals": "APPROVE",
                    "Next": "Approve Application"
                },
                {
                    "Variable": "$.review.decision",
                    "StringEquals": "REJECT",
                    "Next": "Reject Application"
                }
            ]
        },
        "Reject Application": {
            "Type": "Task",
            "Parameters": {
                "id.$": "$.application.id"
            },
            "Resource": "${RejectApplicationFunctionArn}",
            "End": true
        },
        "Approve Application": {
            "Type": "Task",
            "Parameters": {
                "id.$": "$.application.id"
            },
            "Resource": "${ApproveApplicationFunctionArn}",
            "End": true
        }
    }
}
```

![AWS Step Functions](/images/4.3/0001.png?featherlight=false&width=90pc)

1. Thay thế nội dung của file **template.yaml** với nội dung dưới đây.
  + Ấn **Ctrl + S** để lưu thay đổi.
```
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: Template for step-functions-workshop

Resources:
  ApplicationProcessingStateMachine:
    Type: AWS::Serverless::StateMachine
    Properties:
      DefinitionUri: statemachine/account-application-workflow.asl.json
      DefinitionSubstitutions:
        DataCheckingFunctionArn: !GetAtt DataCheckingFunction.Arn
        FlagApplicationFunctionName: !Ref FlagApplicationFunction
        ApproveApplicationFunctionArn: !GetAtt ApproveApplicationFunction.Arn
        RejectApplicationFunctionArn: !GetAtt RejectApplicationFunction.Arn
      Policies:
        - LambdaInvokePolicy:
            FunctionName: !Ref DataCheckingFunction
        - LambdaInvokePolicy:
            FunctionName: !Ref FlagApplicationFunction
        - LambdaInvokePolicy:
            FunctionName: !Ref ApproveApplicationFunction
        - LambdaInvokePolicy:
            FunctionName: !Ref RejectApplicationFunction

  ApproveApplicationFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-ApproveApplication
      CodeUri: functions/account-applications/
      Handler: approve.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable

  DataCheckingFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-DataChecking
      CodeUri: functions/data-checking/
      Handler: data-checking.handler
      Runtime: nodejs12.x

  FindApplicationsFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-FindApplications
      CodeUri: functions/account-applications/
      Handler: find.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable

  FlagApplicationFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-FlagApplication
      CodeUri: functions/account-applications/
      Handler: flag.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable

  RejectApplicationFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-RejectApplication
      CodeUri: functions/account-applications/
      Handler: reject.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable

  ReviewApplicationFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-ReviewApplication
      CodeUri: functions/account-applications/
      Handler: review.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable
        - Statement:
          - Sid: AllowCallbacksToStateMachinePolicy
            Effect: "Allow"
            Action:
              - "states:SendTaskSuccess"
              - "states:SendTaskFailure"
            Resource: !Ref ApplicationProcessingStateMachine

  SubmitApplicationFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: sfn-workshop-SubmitApplication
      CodeUri: functions/account-applications/
      Handler: submit.handler
      Runtime: nodejs12.x
      Environment:
        Variables:
          APPLICATIONS_TABLE_NAME: !Ref ApplicationsTable
          APPLICATION_PROCESSING_STEP_FUNCTION_ARN: !Ref ApplicationProcessingStateMachine
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref ApplicationsTable
        - StepFunctionsExecutionPolicy:
            StateMachineName: !GetAtt ApplicationProcessingStateMachine.Name

  ApplicationsTable:
    Type: 'AWS::DynamoDB::Table'
    Properties:
      TableName: !Sub StepFunctionWorkshop-AccountApplications-${AWS::StackName}
      AttributeDefinitions:
        -
          AttributeName: id
          AttributeType: S
        -
          AttributeName: state
          AttributeType: S
      KeySchema:
        -
          AttributeName: id
          KeyType: HASH
      BillingMode: PAY_PER_REQUEST
      GlobalSecondaryIndexes:
          -
              IndexName: state
              KeySchema:
                  -
                      AttributeName: state
                      KeyType: HASH
              Projection:
                  ProjectionType: ALL
Outputs:
  SubmitApplicationFunctionArn:
    Description: "Submit Application Function ARN"
    Value: !GetAtt SubmitApplicationFunction.Arn
  FlagApplicationFunctionArn:
    Description: "Flag Application Function ARN"
    Value: !GetAtt FlagApplicationFunction.Arn
  FindApplicationsFunctionArn:
    Description: "Find Applications Function ARN"
    Value: !GetAtt FindApplicationsFunction.Arn
  ApproveApplicationFunctionArn:
    Description: "Approve Application Function ARN"
    Value: !GetAtt ApproveApplicationFunction.Arn
  RejectApplicationFunctionArn:
    Description: "Reject Application Function ARN"
    Value: !GetAtt RejectApplicationFunction.Arn
  DataCheckingFunctionArn:
    Description: "Data Checking Function ARN"
    Value: !GetAtt DataCheckingFunction.Arn
```
![AWS Step Functions](/images/4.3/0002.png?featherlight=false&width=90pc)

1. Chạy lệnh dưới đây để thực hiện build và deploy, kiểm tra quá trình deploy thành công trước khi làm bước tiếp theo:
```
sam build && sam deploy
```
![AWS Step Functions](/images/4.3/0003.png?featherlight=false&width=90pc)

Chúc mừng bạn đã hoàn tất tạo một qui trình làm việc cơ bản trên Step Functions state machine.Trong phần tiếp theo chúng ta sẽ tìm hiểu về cách xử lý khi có sự cố xảy ra trong lúc state machine thực thi.

![AWS Step Functions](/images/4.3/0004.png?featherlight=false&width=90pc)
+++
title = "Finish workflow"
date = 2021
weight = 3
chapter = false
pre = "<b>4.3 </b>"
+++


#### Complete the workflow

For now, we still leave the **Approve Application** and **Reject Application** states empty, (using the **Pass** state, not performing a specific action). We will replace these states with Tast states that call the appropriate Lambda functions.

In this step we will:

+ Change the **Approve Application** and **Reject Application** state from **Pass** state to **Task** state and call the appropriate Lambda functions.
+ Grant additional permissions to the IAM Role that Step Functions executes so that it can call the necessary Lambda functions from the **Account Applications** service.

1. Return to the command line interface of the Cloud9 instance, replace the contents of the **statemachine/account-application-workflow.asl.json** file with the content below.

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

1. Replace the content of the file **template.yaml** with the content below.
  + Press **Ctrl + S** to save changes.

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

1. Run the command below to execute the build and deploy, and check the deployment is successful before doing the next step:
```
sam build && sam deploy
```
![AWS Step Functions](/images/4.3/0003.png?featherlight=false&width=90pc)

Congratulations, you've completed creating a basic workflow on the Step Functions state machine. In the next section we'll learn about what to do when something goes wrong while the state machine is running.

![AWS Step Functions](/images/4.3/0004.png?featherlight=false&width=90pc)
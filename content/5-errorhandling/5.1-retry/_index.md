+++
title = "Add retry feature"
date = 2021
weight = 1
chapter = false
pre = "<b>5.1 </b>"
+++


#### Add retry feature

Task states (and others like Parallel state, which we'll talk about later), have the ability to retry work on error. We just need to add a Retry parameter to our Task state definition, which tells the state machine what type of error to retry and optionally specify additional configurations to control retries rate and maximum number of retries. .


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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
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
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
            "End": true
        },
        "Approve Application": {
            "Type": "Task",
            "Parameters": {
                "id.$": "$.application.id"
            },
            "Resource": "${ApproveApplicationFunctionArn}",
            "Retry": [
                {
                    "ErrorEquals": [
                        "Lambda.ServiceException",
                        "Lambda.AWSLambdaException",
                        "Lambda.SdkClientException",
                        "Lambda.TooManyRequestsException"
                    ]
                }
            ],
            "End": true
        }
    }
}
```

![AWS Step Functions](/images/5.1/0001.png?featherlight=false&width=90pc)

1. Run the command below to perform the deploy, check the deployment is successful before doing the next step:
```
sam deploy
```

![AWS Step Functions](/images/5.1/0002.png?featherlight=false&width=90pc)

{{%notice tip%}}
We can specify additional configuration for our Retry parameters, including **IntervalSeconds** (defaults to 1), **MaxAttempts** (defaults to 3) and **BackoffRate** (default is 2), the default configurations are also quite suitable for our case, so we still use the default values.
{{%/notice%}}

Currently, we can't test it because the Lambda function throwing an exception is only temporary. But the purpose of this step is to help provide examples so you know how to manually add retry types to your state machine. Moving on, let's also learn how to handle specific application-level errors.

![AWS Step Functions](/images/5.1/0003.png?featherlight=false&width=90pc)
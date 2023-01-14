+++
title = "Add catch feature"
date = 2021
weight = 2
chapter = false
pre = "<b>5.2 </b>"
+++


#### Add catch

In addition to dealing with temporary problems with Retry, Step Functions also allows us to catch specific errors and respond by transitioning to the appropriate states to handle these errors.

For example, let's say that there are some types of names that our data checking service cannot handle. In these cases, instead of flagging the application for review, we will flag the application as **unprocessable** due to incompatible data.

To illustrate this possibility, we'll leverage some of the testing code in the Lambda function **Data Checking** to tell it to throw an error if it sees a specific string of characters passing in the poster's name. sign.

We'll update our state machine to detect this specific type of custom error and redirect to the new state, and flag the application as **unprocessable**.

In this step we will:
  + Add a new state **Flag Application As Unprocessable** to the state machine.

  + Add Catch configuration to **Check Name** state in our state machine, if encountering data that cannot be processed, it will switch to **Flag Application As Unprocessable** state.
  
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
            "Catch": [
                {
                    "ErrorEquals": [
                        "UnprocessableDataException"
                    ],
                    "ResultPath": "$.error-info",
                    "Next": "Flag Application As Unprocessable"
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
        },
        "Flag Application As Unprocessable": {
            "Type": "Task",
            "Resource": "arn:aws:states:::lambda:invoke",
            "Parameters": {
                "FunctionName": "${FlagApplicationFunctionName}",
                "Payload": {
                    "id.$": "$.application.id",
                    "flagType": "UNPROCESSABLE_DATA",
                    "errorInfo.$": "$.error-info"
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
            "End": true
        }
    }
}
```

![AWS Step Functions](/images/5.2/0001.png?featherlight=false&width=90pc)

1. Run the command below to perform the deploy, check the deployment is successful before doing the next step:
```
sam deploy
```

![AWS Step Functions](/images/5.2/0002.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/5.2/0003.png?featherlight=false&width=90pc)

1. Run the command below to submit a new application that contains unprocessable data for the applicant's name field.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "UNPROCESSABLE_DATA", "address": "123 Street" }' /dev/stdout
```



![AWS Step Functions](/images/5.2/0004.png?featherlight=false&width=90pc)

1. Return to the Step Functions interface, Click on the state machine name **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.


![AWS Step Functions](/images/5.2/0005.png?featherlight=false&width=90pc)

2. Click on the latest execution.


![AWS Step Functions](/images/5.2/0006.png?featherlight=false&width=90pc)

3. We can see our state machine has detected unprocessable data and passed the state to **Flag Application As Unprocessable**.


![AWS Step Functions](/images/5.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/5.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/5.2/0009.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/5.2/00010.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/5.2/00011.png?featherlight=false&width=90pc)

4. We can check the application is flagged as unprocessable by running the command below.
```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_WITH_UNPROCESSABLE_DATA" }' /dev/stdout
```
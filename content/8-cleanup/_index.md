+++
title = "Clean up resources"
date = 2021-07-09T14:40:59+07:00
weight = 8
chapter = false
pre = "<b>8. </b>"
+++

You would clean up resources in the following order:

1. Access the management interface of [AWS Step Functions](https://console.aws.amazon.com/states/home?region=ap-southeast-1).
  + Click on state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
  + Click on the execution that is running.

![AWS Step Functions](/images/8/0001.png?featherlight=false&width=90pc)

1. Click **Stop execution** to stop the execution of the state machine.

![AWS Step Functions](/images/8/0002.png?featherlight=false&width=90pc)

{{%notice warning%}}
Make sure no more executions are in **running** state before you proceed to the next step. AWS Cloud Formation will not perform state machine deletion if there is still execution running.
{{%/notice%}}

1. Return to the Cloud9 Instance interface. run the following command to delete the AWS CloudFormation stack that AWS SAM created for our application.
```
REGION=$(grep region samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
STACK_NAME=$(grep stack_name samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
aws cloudformation delete-stack --region $REGION --stack-name $STACK_NAME
```
![AWS Step Functions](/images/8/0003.png?featherlight=false&width=90pc)

1. Access the management interface [Cloud9 service](https://ap-southeast-1.console.aws.amazon.com/cloud9/home?region=ap-southeast-1)
  + Select Cloud9 Instance **StepFunctions**.
  + Click **Delete**.

![AWS Step Functions](/images/8/0004.png?featherlight=false&width=90pc)

2. Enter **Delete** to confirm.
  + Click **Delete** to proceed to delete Cloud9 Instance.

![AWS Step Functions](/images/8/0005.png?featherlight=false&width=90pc)
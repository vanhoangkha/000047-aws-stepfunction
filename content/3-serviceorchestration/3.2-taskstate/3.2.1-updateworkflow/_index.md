+++
title = "Update workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.2.1 </b>"
+++


#### Update workflow

1. Go to the [AWS Step Functions interface](https://console.aws.amazon.com/states/home?region=ap-southeast-1).
  + Click **Process_New_Account_Applications**.
  + Click **Edit**.

![AWS Step Functions](/images/3.2.1/0001.png?featherlight=false&width=90pc)

2. Next, we will update the definition of the state machine. Note that after pasting the content below, you'll see a few lines with an error indicator because our new state machine definition has some placeholder strings written as 'REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN'. **We will fix this in the next step**. Replace the existing definition with the following:
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
            "Resource": "REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN",
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
            "Resource": "REPLACE_WITH_DATA_CHECKING_LAMBDA_ARN",
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}

```
![AWS Step Functions](/images/3.2.1/0002.png?featherlight=false&width=90pc)

1. Return to the Cloud9 Terminal interface, run the command below, to determine the **ARN** of **DataCheckingFunction**. (Make sure you are in the **workshop-dir** folder location. )

```
REGION=$(grep region samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
STACK_NAME=$(grep stack_name samconfig.toml | awk -F\= '{gsub(/"/, "", $2); gsub(/ /, "", $2); print $2}')
aws cloudformation describe-stacks --region $REGION --stack-name $STACK_NAME --query 'Stacks[0].Outputs[?OutputKey==`DataCheckingFunctionArn`].OutputValue' --output text
```
{{%notice tip%}}
The above seemingly complicated set of commands is just to help you get the ARN faster by automatically pulling the configuration values ​​out of the samconfig.toml file (this file remembers things like Region and the name of the cloudformation stack we're using. SAM for deployment), then use those profiles to configure the value for the AWS CLI command to display the ARN of the Lambda functions **DataCheckingFunction** we deployed.
{{%/notice%}}

![AWS Step Functions](/images/3.2.1/0003.png?featherlight=false&width=90pc)

1. Copy and save the result of step 3.

![AWS Step Functions](/images/3.2.1/0004.png?featherlight=false&width=90pc)

2. In the **Definition** section of the state machine, update the Lambda function ARN parameter of step 3 as shown below:
  + Click **Save**.

![AWS Step Functions](/images/3.2.1/0005.png?featherlight=false&width=90pc)

3. We get a warning that our IAM role may need to change to allow the state machine to execute. This is a helpful reminder. Actually, we have changed our state machine and will ask to change permissions. Now, we require the Lambda function **Data Checking** capability. We will deal with this in the next big step. Click **Save anyway**.
+++
title = "Update workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.5.1 </b>"
+++


#### Update workflow

The steps we will do in this section:
+ Add a new status called **Pending Review**.
+ Add status **Review Required ?** Use **Choice state** to switch to **Pending Review** state if checking the name or address returns flagged results.
+ Update **Check Address** step to switch to **Review Required?** status.

1. Return to the command line interface of the Cloud9 instance, and replace the contents of the file **state-machine/account-application-workflow.asl.json** with the content below.
  + Press **Ctrl + S** to save changes.

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
            "Type": "Pass",
            "End": true
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
 ```

![AWS Step Functions](/images/3.5.1/0001.png?featherlight=false&width=90pc)

1. Perform the update deployment with the following command:
```
sam deploy
```

![AWS Step Functions](/images/3.5.1/0002.png?featherlight=false&width=90pc)

1. Check the successful update deployment.

![AWS Step Functions](/images/3.5.1/0003.png?featherlight=false&width=90pc)

We just added two new statuses to our workflow: **Review Required** and **Pending Review**. **Review Requried state?** Checks its input (which is the output from **Check Address** state) and will run through a series of checks ( rules ).

{{%notice tip%}}
You can see that there are a series of two selection rules in the definition of the state, each specifying the state to go to the next if its rule matches successfully. There is also a default state name specified to switch to in case there are no matching rules.
{{%/notice%}}

One of our Selection rules says that if the value inside the input at checks.name.flagged is true, the next state will be **Pending Review**. The other selection rule is the same, check checks.address.flagged to see if it is true, in which case it goes to **Pending Review** as well. Finally, the default value of our **Choice state** indicates that if no selection rule matches, the state machine will transition to the **Approve Application** state.

The next step we will try to execute the workflow to see the results.


{{%notice info%}}
If you're more interested in the behavior and comparison types supported by **Choice state**, check out the developer guide at [https://docs.aws.amazon.com/step-functions /latest/dg/amazon-states-language-choice-state.html](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-choice-state.html)
{{%/notice%}}
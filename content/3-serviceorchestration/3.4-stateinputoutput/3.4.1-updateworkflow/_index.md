+++
title = "Update workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.4.1 </b>"
+++


#### Update workflow

In this step we will add ResultPath property to **Check Name** and **Check Address** state inside our machine state defined in **state-machine/account-application-workflow.asl .json**.

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
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
 ```

![AWS Step Functions](/images/3.3.4/0001.png?featherlight=false&width=90pc)

2. Perform the update deployment with the following command:
```
sam deploy
```

![AWS Step Functions](/images/3.3.4/0002.png?featherlight=false&width=90pc)

3. Check the successful update deployment.

![AWS Step Functions](/images/3.3.4/0003.png?featherlight=false&width=90pc)

In the next step we will try to execute the workflow to see the results.
+++
title = "Initialize workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1.1 </b>"
+++


#### Initiate workflow

To get started, let's try to model the steps involved to check the name, check the address, and then approve the application. Our simple workflow would start like this:

![StepFunctions](/images/SF/020.png?width=40pc)

1. Go to the [AWS Step Functions interface](https://console.aws.amazon.com/states/home?region=ap-southeast-1).

2. Click Menu to expand the menu of Step Functions.

![AWS Step Functions](/images/3.1.1/0001.png?featherlight=false&width=90pc)

3. Click **State machines**, then click **Create state machine**.

![AWS Step Functions](/images/3.1.1/0002.png?featherlight=false&width=90pc)

4. Click **Write your workflow in code**.
  + Drag the screen down to the **Definition** section and replace the content with the JSON snippet below:

```
{
    "StartAt": "Check Name",
    "States": {
        "Check Name": {
            "Type": "Pass",
            "Next": "Check Address"
        },
        "Check Address": {
            "Type": "Pass",
            "Next": "Approve Application"
        },
        "Approve Application": {
            "Type": "Pass",
            "End": true
        }
    }
}
```

![AWS Step Functions](/images/3.1.1/0003.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.1/0004.png?featherlight=false&width=90pc)

5. We can see the updated workflow as shown below. Click **Next** to continue.

6. In the Name section, name the state machine **Process_New_Account_Applications**.
  + In the Permissions section, we will need to specify the IAM role for the Step Functions assumed when executing. We'll start with the default role. Click on **Create new role**.

![AWS Step Functions](/images/3.1.1/0005.png?featherlight=false&width=90pc)

7. Leave the rest of the options as default, scroll down and click **Create state machine**.

![AWS Step Functions](/images/3.1.1/0006.png?featherlight=false&width=90pc)

{{%notice tip%}}
In **AWS Step Functions**, we define our **state machines** using a JSON-based structured language called Amazon States Language. You can read more about the full language specification and all supported state types at https://states-language.net/spec.html
{{%/notice%}}

8. State machine is created successfully.

![AWS Step Functions](/images/3.1.1/0007.png?featherlight=false&width=90pc)
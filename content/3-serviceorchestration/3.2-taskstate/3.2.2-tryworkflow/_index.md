+++
title = "Check the workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2.2 </b>"
+++


#### Check workflow
The warning we see as soon as we update our state machine definition is correct. We will need to update our IAM role permissions for the state machine to work. But anyway let's try another execution to see what the insufficient permission error will look like.

1. Click **Start execution**.

![AWS Step Functions](/images/3.2.2/0001.png?featherlight=false&width=90pc)

2. Paste the JSON content below into the input. Then click **Start execution**.
```
{
    "application": {
        "name": "Spock",
        "address": "123 Enterprise Street"
    }
}

```
![AWS Step Functions](/images/3.2.2/0002.png?featherlight=false&width=90pc)

{{%notice tip%}}
After a while, you will see the result of this failed execution. The "Execution status" displays "Failed" and you'll see a red alert in the state machine process representation below, highlighting the failed status.
{{%/notice%}}

![AWS Step Functions](/images/3.2.2/0003.png?featherlight=false&width=90pc)

3. Click the **Check Name** state, then click the **Exception** tab. We can see the error details as shown below:


![AWS Step Functions](/images/3.2.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.2.2/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.2.2/0006.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.2.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.2.2/0008.png?featherlight=false&width=90pc)

When the state machine executes, it assumes the IAM role to determine what types of actions it is allowed to perform within the AWS environment. Currently, we haven't added any explicit permissions to allow this Role to call our Lambda function **Data Checking**, so we get an error when this state machine tries to run.

We will work around this by adding the appropriate permissions to the Role that our Step Functions assume during execution in the next step.
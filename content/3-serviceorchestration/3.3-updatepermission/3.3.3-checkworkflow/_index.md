+++
title = "Check the workflow"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3.3 </b>"
+++


#### Check workflow


1. Go to [State machines interface](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click on state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
  This is a re-implemented version of our state machine. The new version has not changed, except that we have granted its IAM role to call the lambda function **Data Checking**. Try doing it again with some sample input to see what happens.
 
![AWS Step Functions](/images/3.3.3/0001.png?featherlight=false&width=90pc)

2. Click **Start execution** to execute the workflow again.
3. Enter the following JSON content in the input field.
```
{
    "application": {
        "name": "Spock",
        "address": "123 Enterprise Street"
    }
}
```
  + Click **Start execution**.

![AWS Step Functions](/images/3.3.3/0002.png?featherlight=false&width=90pc)

{{%notice tip%}}
After a while, you will see that the execution failed. This time, however, we don't have any red states, because this time we get a different error. \
**Check Name** status has executed successfully, however **Check Address** status is grayed out. If you look at the color code at the bottom of the visualization you will see that this means the state has been destroyed. Let's see why.
{{%/notice%}}

![AWS Step Functions](/images/3.3.3/0003.png?featherlight=false&width=90pc)

1. Scroll down to the **Execution event history** section.
 + Click on the last execution.
 + We will see the error as below
 ```
 {
  "error": "States.Runtime",
  "cause": "An error occurred while executing the state 'Check Address' (entered at the event id #7). The JSONPath '$.application.address' specified for the field 'address.$' could not be found in the input '{\"flagged\":false}'"
}
```

![AWS Step Functions](/images/3.3.3/0004.png?featherlight=false&width=90pc)


 If you look back at our state machine definition for the **Check Address** state, you'll see that it expects an **application** object in its input and it tries to pass **application .address** down to Lambda function **Data Checking**.
 
![AWS Step Functions](/images/3.3.3/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0006.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.3.3/0008.png?featherlight=false&width=90pc)

The error message tells us that the state machine cannot find application.address in the state input. To understand why, we need to learn a little more about how an active state produces its output and passes it to the next state's input in the next step.
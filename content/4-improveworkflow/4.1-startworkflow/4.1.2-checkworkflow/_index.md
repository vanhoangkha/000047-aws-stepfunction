+++
title = "Check workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>4.1.2 </b>"
+++


#### Check workflow


1. Run the following statement to execute the Lambda function that submits an application that contains invalid address information.

 ```
 aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "AnInvalidAddress" }' /dev/stdout
 ```
![AWS Step Functions](/images/4.1.2/0001.png?featherlight=false&width=90pc)

2. Go to [State machines interface](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click on state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.

![AWS Step Functions](/images/4.1.2/0002.png?featherlight=false&width=90pc)

3. In the **Executions** tab, click on the last execution.

![AWS Step Functions](/images/4.1.2/0003.png?featherlight=false&width=90pc)

4. Click state **Pending Review**, then click **Step output**.
  + We can see the application ID attribute has been added.

![AWS Step Functions](/images/4.1.2/0004.png?featherlight=false&width=90pc)

So we have successfully passed the application ID into Step Functions, we are ready to configure the **Pending Review** state to notify the **Account Applications** service when an application is tied flag and will pause state machine execution to wait for the controller to re-check the registration information.

![AWS Step Functions](/images/4.1.2/0005.png?featherlight=false&width=90pc)
+++
title = "Check workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.4.2 </b>"
+++


#### Check workflow


1. Go to [State machines interface](https://ap-southeast-1.console.aws.amazon.com/states/home?region=ap-southeast-1#/statemachines)
  + Click on state machine **ApplicationProcessingStateMachine-xxxxxxxxxxxx**.
 
![AWS Step Functions](/images/3.4.2/0001.png?featherlight=false&width=90pc)

2. Click on the last execution of the state machine. ( is being failed )
 + Click **New execution** to execute a new time.

![AWS Step Functions](/images/3.4.2/0002.png?featherlight=false&width=90pc)

3. Leave the input as it is and click **Start execution**.

![AWS Step Functions](/images/3.4.2/0003.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0004.png?featherlight=false&width=90pc)

4. We should see the workflow executed successfully.
  + Click state **Check Address**.
  + Click **Step Input**.

![AWS Step Functions](/images/3.4.2/0005.png?featherlight=false&width=90pc)

5. Click **Step Output** to check the output.

![AWS Step Functions](/images/3.4.2/0006.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.4.2/0008.png?featherlight=false&width=90pc)


{{%notice tip%}}
Notice how **Check Name** state holds our original input and adds its result inside **$ .checks.name** and how **Check Address** takes that output as input and add its own address check result inside **$ .checks.address**. That's the power of ResultPath.
{{%/notice%}}

At this point, we have a successful workflow, but the workflow is still missing some important logic. Our workflow goes directly from **Check Address** to **Approve Application**. What we want is to automatically approve an application only if both the name and address are not flagged, and if the application is flagged we will queue the application for the controller to do the re-check.

We will incorporate the step of waiting for a controller response on flagged applications. But before that, we need to learn how to check the status of the working workflow and execute some branching logic based on the checks we define. To do this, we will need to add a new type of state to our state machine called Choice state.
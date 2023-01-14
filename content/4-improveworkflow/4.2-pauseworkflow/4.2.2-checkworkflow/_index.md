+++
title = "Check the workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>4.2.2 </b>"
+++


#### Check workflow
We can now submit an invalid application, the application will be flagged for review, approval or rejection by the controller and then the review results will be returned to the state machine for further processing. continue to execute the procedure.

1. Run the command below to submit a registration with an invalid address.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "InvalidAddressFormat" }' /dev/stdout
```
![AWS Step Functions](/images/4.2.2/0001.png?featherlight=false&width=90pc)

1. Check if our subscription is flagged for rechecking with the command below
```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_FOR_REVIEW" }' /dev/stdout

```
![AWS Step Functions](/images/4.2.2/0002.png?featherlight=false&width=90pc)

1. Scan the block and copy the id of the flagged application.

![AWS Step Functions](/images/4.2.2/0003.png?featherlight=false&width=90pc)

2. Back to the Step Functions service interface, click on State machine **ApplicationProcessingStateMachine-xxxxxxx**.

![AWS Step Functions](/images/4.2.2/0004.png?featherlight=false&width=90pc)

3. At the Executions tab, we can see the status of this execution is showing as **Running**. Click to select the latest execution.
  + We will see the status **Pending Review** is **in-progress**. This means that our state machine is paused and waiting for a response before resuming execution.

![AWS Step Functions](/images/4.2.2/0005.png?featherlight=false&width=90pc)

4. Back in the Cloud9 console, we'll need to respond and approve the application. (We haven't built a web interface for this yet, we just call a Lambda Function in the **Account Applications** service to perform the approve operation.)
  + Run the command below and replace **REPLACE_WITH_APPLICATION_ID** with the registration ID value you copied in step 3.

```
aws lambda invoke --function-name sfn-workshop-ReviewApplication --payload '{ "id": "REPLACE_WITH_APPLICATION_ID", "decision": "APPROVE" }' /dev/stdout
```

![AWS Step Functions](/images/4.2.2/0006.png?featherlight=false&width=90pc)

1. Returning to the Step Functions interface we can see that the execution has been continued and completed (since we have approved the application).
  + Click on state **Review Approved?**.
  + Click on **Step input**.
  + We can see the approval decision has been included in the input data. (via the **SendTaskSuccess** response that the Lambda function **Review Application** - functions/account-Applications/review.js called).

![AWS Step Functions](/images/4.2.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/0009.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/00010.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/4.2.2/00011.png?featherlight=false&width=90pc)

To complete the workflow implementation, we will replace the **Approve Application** and **Reject Application** steps to call the Lambda functions ApproveApplication and RejectApplication that we created.
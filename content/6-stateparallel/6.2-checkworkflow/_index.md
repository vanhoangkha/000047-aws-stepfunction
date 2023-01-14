+++
title = "Check workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>6.2 </b>"
+++


#### Check workflow

We'll try submitting different applications to see how our state machine performs.

1. Run the command below to submit a valid registration.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "123 Enterprise Street" }' /dev/stdout
```
**ApplicationProcessingStateMachine-xxxxxxxxxxxx**/

![AWS Step Functions](/images/6.2/0001.png?featherlight=false&width=90pc)

1. Back to StepFunctions interface, click on state machine

![AWS Step Functions](/images/6.2/0002.png?featherlight=false&width=90pc)

2. At the Executions tab, click on the last execution.

![AWS Step Functions](/images/6.2/0003.png?featherlight=false&width=90pc)


3. We can see **Check Name** and **Check Address** states executed in parallel.

![AWS Step Functions](/images/6.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0006.png?featherlight=false&width=90pc)

4. Run the command below to submit an invalid registration.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "ABadAddress" }' /dev/stdout
```
![AWS Step Functions](/images/6.2/0007.png?featherlight=false&width=90pc)

1. Do the same steps 2-4, we will see our state machine stop at **Pending Review** step.

![AWS Step Functions](/images/6.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/0009.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/00010.png?featherlight=false&width=90pc)

2. Run the command below to submit a subscription whose data cannot be processed.

```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "UNPROCESSABLE_DATA", "address": "123 Street" }' /dev/stdout
```

![AWS Step Functions](/images/6.2/00011.png?featherlight=false&width=90pc)

1. Do the same steps 2-4, we will see our state machine stop at **Flag Application as Unprocessable** step.

![AWS Step Functions](/images/6.2/00012.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/6.2/00013.png?featherlight=false&width=90pc)

Congratulations on completing the workshop, we have a well-structured state machine to manage the processing of new registrations for a simple banking system. If desired, we can add one more step in our workflow to handle even more detailed logic related to opening a bank account for approved applications. Hopefully, through this workshop, you have gained the necessary experience to continue to develop applications on your own with AWS Step Functions.
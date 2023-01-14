+++
title = "Check workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.5.2 </b>"
+++


#### Check workflow


1. Return to the Step Functions interface.
  + Click **New execution**.
 
![AWS Step Functions](/images/3.5.2/0001.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.5.2/0002.png?featherlight=false&width=90pc)

2. Try to create a valid application using json input like below:
```
{ "application": { "name": "Spock", "address": "123 Enterprise Street" } }
```
  + Click **Start execution**.

![AWS Step Functions](/images/3.5.2/0003.png?featherlight=false&width=90pc)

3. Check status **Review Required?** has changed to **Approve Application** , because both v2a name and address contain valid information.

![AWS Step Functions](/images/3.5.2/0004.png?featherlight=false&width=90pc)

4. Click **New execution** to execute a new execution.

![AWS Step Functions](/images/3.5.2/0005.png?featherlight=false&width=90pc)

5. Try to create an invalid application using json input like below:
```
{ "application": { "name": "Spock", "address": "Somewhere" } }
```
  + Click **Start execution**.

![AWS Step Functions](/images/3.5.2/0006.png?featherlight=false&width=90pc)

6. Notice how we routed to **Pending Review**, this time we're going to an invalid address in the pattern <number>-<space>-<text>. (ex: 123 Street)

![AWS Step Functions](/images/3.5.2/0007.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.5.2/0008.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.5.2/0009.png?featherlight=false&width=90pc)

Thanks to **Choice state**, we are now routing our workflow the way we want. However, we have not done any processing in the **Approve Application** and **Pending Review** states yet. We're pausing the Application Approval step until later in the workshop (we already know how to integrate with a Lambda function, so that's not an exciting next step to take). Instead, we'll keep the momentum going and learn how to implement the **Pending Review** state in the next step.
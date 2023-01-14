+++
title = "Pause workflow to wait for the test to complete"
date = 2021
weight = 2
chapter = false
pre = "<b>4.2 </b>"
+++


#### Pause the workflow to wait for the test to complete

**Step Functions** does its job by integrating directly with various AWS services, and you can control these AWS services using the following three service integrations:

  Call a service and let Step Functions move to the next state as soon as it receives the HTTP response. You've seen this kind of integration in action through what we use to call the Lambda function that checks the data and gets back a response.

  + Call a service and let Step Functions wait for the job to complete. This is typically used to trigger workloads in a batch fashion, pause, then resume execution once the job is complete. We are not using this integration in the current workshop.

  + Call a service that has a task token and let Step Functions wait until that token is returned along with the payload. This is the integration we want to use here, because we want to make a service call, then wait for a response, and then continue executing.

{{%notice info%}}

The response ( call back ) provides a way to pause a workflow until a task token is transferred again. A task may need to wait for controller approval, integrate with a third party, or invoke legacy systems. For tasks like this, you can pause the execution of the Step Functions state machine and wait for an external process or workflow to complete.
{{%/notice%}}

You can modify the Task state to generate a unique task token (a unique ID that references a specific Task state during a particular execution), call the desired AWS service, and then suspend execution. executes until the Step Functions service receives the task token back through an API call from some other process.

In this step we will:

  + Setting our **Pending Review** state calls the Lambda function **Account Applications** using the Task state definition syntax including the .waitForTaskToken suffix. This will generate a task token that we can pass to the **Account Applications** service along with the registration ID we want to flag.

+ Make the **Account Applications** Lambda function update the registration record to mark it as **Pending Review** and store the task token with the record. Then, when the controller considers the pending registration, the **Account Applications** service executes the Step Functions API callback, calls the **SendTaskSuccesss** endpoint, passing the task token back with the output. relevant from this step, in our case, will be the data to indicate whether the controller has approved or denied the registration. This information will allow the state machine to decide what to do next based on the controller's decision.

  + Create a new Lambda function **ReviewApplication** (and the required IAM permissions for it) to implement logic that allows the controller to make a decision for a flagged booking, calling the Step Functions API again with the result is the decision of the controller.

  + Add another Selection state named **Review Approved?** which will check the result from **Pending Review** and switch to **Approve Application** or **Reject Application status ** which we will also add .

#### Content
1. [Update state machine](4.2.1-updateworkflow/)
2. [Checkworkflow](4.2.2-checkworkflow/)
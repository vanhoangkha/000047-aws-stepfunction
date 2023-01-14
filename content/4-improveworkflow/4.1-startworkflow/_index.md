+++
title = "Activate workflow"
date = 2021
weight = 1
chapter = false
pre = "<b>4.1 </b>"
+++


#### Activate workflow

In the **Pending Review** state, we want the state machine to call the **Account Applications** service to flag the subscriptions that need rechecking, then pause and wait for the callback from the service ** Account Applications**, this will happen after the controller checks and makes a decision.

In order for our Step Functions to notify the **Account Applications** service that a subscription will be flagged, you need to pass it a subscription ID. And the way Step Functions can pass an ID back to the **Account Applications** service is to **implement the ID attribute as part of the registration information at the start of workflow execution.**

To do this, we will integrate the **Account Applications** service with the Step Functions that process the application, starting a new execution every time a new application is submitted to the service. When we start the execution, in addition to passing the user's name and address as input (to check the name and address), we will also pass the registration ID so that Step Functions can execute the function ** FlagApplication** of the **Account Applications** service to flag applications for review.

In this step we will:
  + Pass ARN of state machine as environment variable for lambda function **SubmitApplication**.

  + Updated Lambda function **SubmitApplication** to execute our Step Functions state machine when a new application is submitted, passing the subscriber details into the state machine input.

  + Grant the Lambda function **SubmitApplication** permission to execute our state machine.

#### Content
1. [Update state machine](4.1.1-updateworkflow/)
2. [Checkworkflow](4.1.2-checkworkflow/)
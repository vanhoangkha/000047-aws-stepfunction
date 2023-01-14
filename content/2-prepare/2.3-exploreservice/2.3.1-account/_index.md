+++
title = "Account Application"
date = 2021
weight = 1
chapter = false
pre = "<b>2.3.1 </b>"
+++

#### Check **Account Application** service.

Using AWS SAM CLI, we can call any Lambda Function in our service with parameters depending on what we want to do. Let's try to run these commands one by one to understand what we can do with applications or user data.

Below, we are using the AWS CLI, via **aws lambda invoke**, to directly call a deployed AWS Lambda Function with the desired content. We haven't started using AWS Step Functions yet. Here, we're just testing the Lambda Functions that we'll start organizing into a workflow using StepFunctions in this workshop.

1. Run the command below in the command line interface to submit a new registration.
```
aws lambda invoke --function-name sfn-workshop-SubmitApplication --payload '{ "name": "Spock", "address": "123 Enterprise Street" }' /dev/stdout
```
![AWS Step Functions](/images/2.3.1/0001.png?featherlight=false&width=90pc)

2. Copy the application ID, in the result of the above command to use for the next step.
  + Example: **application_869a2256-fe54-4731-9522-4cbbc1a184ad**

3. Flag a registration to be inspected by a controller. (note to replace **REPLACE_WITH_ID** with the ID value of the application you saved in step 2.)
```
aws lambda invoke --function-name sfn-workshop-FlagApplication --payload '{ "id": "REPLACE_WITH_ID", "flagType": "REVIEW" }' /dev/stdout
```
  + Example command after changing REPLACE_WITH_ID.
  ```
  aws lambda invoke --function-name sfn-workshop-FlagApplication --payload '{ "id": "application_869a2256-fe54-4731-9522-4cbbc1a184ad", "flagType": "REVIEW" }' /dev/stdout
  ```

![AWS Step Functions](/images/2.3.1/0002.png?featherlight=false&width=90pc)

4. List all subscriptions currently flagged for review.

```
aws lambda invoke --function-name sfn-workshop-FindApplications --payload '{ "state": "FLAGGED_FOR_REVIEW" }' /dev/stdout
```

![AWS Step Functions](/images/2.3.1/0003.png?featherlight=false&width=90pc)

5. Approve a flagged application: (note to replace **REPLACE_WITH_ID** with the application ID value you saved in step 2.)

```
aws lambda invoke --function-name sfn-workshop-ApproveApplication --payload '{ "id": "REPLACE_WITH_ID" }' /dev/stdout
```
  + Example command after changing REPLACE_WITH_ID.
  ```
  aws lambda invoke --function-name sfn-workshop-ApproveApplication --payload '{ "id": "application_869a2256-fe54-4731-9522-4cbbc1a184ad" }' /dev/stdout
  ```

![AWS Step Functions](/images/2.3.1/0004.png?featherlight=false&width=90pc)
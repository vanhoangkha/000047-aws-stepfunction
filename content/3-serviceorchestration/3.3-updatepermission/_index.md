+++
title = "Update Permissions"
date = 2021
weight = 3
chapter = false
pre = "<b>3.3 </b>"
+++


#### Update Permissions

Instead of continuing to work in the web console and performing manual debugging, we will use the AWS SAM tool to define the state machine along with other resources used in this workshop and set permissions for This state machine executes successfully.

The main steps we will do:
  Declare our state machine inside a new file at statemachine/ account-application-workflow.asl.json

  + Updated our SAM template file template.yaml to include the new state machine deployment resource.

  Add a new IAM role for the state machine to take on as it executes. The role that grants permission to the state machine calls the Lambda function **Data Checking**.

{{%notice tip%}}
Before we move the state machine definition to the template.yaml file, we delete the state machine in the Step Functions console so that there is no confusion when a similar state machine is deployed.
{{%/notice%}}

#### Content
1. [Delete state machine](3.3.1-deleteoldworkflow/)
2. [Update SAM template](3.3.2-updatesamtemplate/)
3. [Checkworkflow](3.3.3-checkworkflow/)
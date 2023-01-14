+++
title = "Workflow details"
date = 2021
weight = 3
chapter = false
pre = "<b>1.3 </b>"
+++


#### Workflow details

Whenever there is a new account registration in our sample system, we will do some checks of the registrant's information and then automatically approve the application if there is no problem with the application. data or flag it for review by controllers. The controller will periodically review flagged applications and decide whether to approve or deny the application.

In a nutshell, here's the workflow we'll be managing:

1. Check the name and address of a registrant for a service to detect if they are suspicious or need a controller before processing an account application.
2. If the name and address are checked without any problems, the application will be automatically approved. If you get an error when you try to check the name or address, flag the application as unprocessable and stop it. Otherwise, if checking the name or address shows something amiss in the data, continue to the next step.

3. Flag the application for processing by the controller and suspend further processing.

4. Wait for a controller to examine the flagged application and issue an approval or denial decision.

5. Approve or reject the application, at the discretion of the Controller.

Here is a diagram to illustrate our workflow:

![StepFunctions](/images/SF/002.png?width=60pc)
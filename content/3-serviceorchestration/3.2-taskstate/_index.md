+++
title = "Task State Type"
date = 2021
weight = 2
chapter = false
pre = "<b>3.2 </b>"
+++
#### Task State Style
**Task state** causes the interpreter to do the work specified by the state's **Resource** field. Below, we will use **Task** states to call the Lambda function of the **Data Checking** service, passing the name and address properties from the application into Lambda calls functions.

In the Task states below, in addition to specifying the ARN of the Lambda function that we want the Task state to use, we also include the Parameters section. Here's how to pass data from the state input into the target Lambda function's input. You'll notice that you're using a syntax that includes dollar signs at the end of each property name and the beginning of each attribute value. This is to let Step Functions know we want to include state data in these parameters instead of sending a raw string of $.application.name for example.

The state machine used below assumes that it will receive an initial input object with a single property called **application** (representing registration) which has a name (name) and address (address) properties. ). We'll specify this input to execute shortly after we update our state machine definition.

#### Content
1. [Update Workflow](3.2.1-updateworkflow/)
2. [Check Workflow](3.2.2-tryworkflow/)
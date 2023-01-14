+++
title = "State Input/Output"
date = 2021
weight = 4
chapter = false
pre = "<b>3.4 </b>"
+++


#### State Input/Output

Each execution of the state machine takes a JSON document as input and passes that input to the first state in the workflow. Individual states receive JSON as input and pass JSON as output to the next state. Understanding how this information moves from one state to another and learning how to filter and manipulate this data, are key to effectively designing and implementing workflows in AWS Step Functions.

You can refer to the [Input and Output Processing section of the developer guide](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-input-output- filtering.html) for detailed information, for now we will only cover the knowledge required for our state machine to work.

The output of a state can be a copy of the input or result it produces (for example, the output from a Lambda Function of the state **Task** ) or a combination of the input and its result. We can use the **ResultPath** attribute inside our state machine task definitions to control which combination of these result configurations is passed to state output (which then becomes input). enter for the next state).

{{%notice tip%}}
The reason the execution failed in step 3.3 is that **the default behavior of a Task, if the ResultPath property is not specified, is to take the output of the task and use it as input for the next state. follow**. In our case, since the previous state (Check Name) produced {"flagged": false} output, it became the input for the next state (Check Address). \
\
What we want to do is keep the original input, contain the subscriber's information, merge the results of **Check Name** into that state, and move the entire content down to **Check Address** . Then **Check Address** can do the same. What we want to do is make both data checks execute correctly and merge their outputs together.

{{%/notice%}}

 
So to fix the current problem, we need to add a ResultPath statement, which instructs the state machine to generate its output by taking the output of the Lambda function and merging it with the input of the state. It's a simple change. We just need to add a little extra configuration to the definition of the Task state: **"ResultPath": "$.SomePropertyName"**.

In Amazon States Language, the dollar sign syntax you see here means input of state. We'll put the result of executing this action (in this case, the output of the Lambda function) into a new property of the object containing the input state and use it as the output of the state.
{{%notice info%}}
 For example: **"ResultPath": "$.checks.name"** , i.e. put the result of state **Check Name** into a new property named **checks.name** of the containing object input state.
{{%/notice%}}

#### Content
1. [Update state machine](3.4.1-updateworkflow/)
2. [Checkworkflow](3.4.2-checkworkflow/)
+++
title = "Style selection status"
date = 2021
weight = 5
chapter = false
pre = "<b> 3.5 </b>"
+++


#### Style Selection Status

**Selection state** adds logical branching to the state machine. You can think of this as a common statement variable in many language programs. **Selection state** has a bunch of rules (**rules**). Each rule contains two things: an expression that evaluates to some boolean expression, and a reference to the next state to switch to if the rule matches successfully.

All rules are evaluated in order, and the first one that succeeds causes the state machine to transition to the next state defined by the rule.

In our sample workflow, we want an inspector to check an application if two states ** Check Name ** and ** Check Address ** return flags. Otherwise, we want to automatically approve the app. Please add to ** Choice State ** to implement this workflow.

Here's an update to our workflow we'll see once we're done with this step:

! [StepFunctions] (/images/SF/a01.png? Width = 50pc)

#### Content
1. [Update machine state] (3.5.1-updateworkflow/)
2. [Test Workflow] (3.5.2-Test Procedure/)
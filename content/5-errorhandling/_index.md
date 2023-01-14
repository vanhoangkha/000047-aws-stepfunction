+++
title = "Error handling"
date = 2021
weight = 5
chapter = false
pre = "<b>5. </b>"
+++


#### Handle errors during workflow execution by adding retry and error handling (catch) capabilities

Up to this point our workflow is not capable of handling any errors. What if some Lambda function calls timeout or if they encounter some other kind of transient error? What if Lambda functions throw exceptions? Or one of the Lambda functions is calling a third-party service. That external call may also fail or timeout. Together we'll tackle these things-ifs and take advantage of AWS Step Functions' built-in retry and error handling - catch.

So what kind of errors can occur? Here are some reference errors:

+ Any state can get a runtime error. Errors can occur for many reasons:
  State machine definition problems (e.g. no matching rules in Selection state)
  + Task error (eg Lambda function exception)
  + Temporary problems (e.g. network problems)

By default, when an error status occurs, AWS Step Functions causes the execution to fail completely.

For our sample workflow, we can agree to allow our workflow to fail when any unexpected error occurs. But some Lambda function call errors are temporary, so we should at least add some retry behavior to the Task state that calls Lambda functions.

#### Content
1. [Add Retry Feature](5.1-retry/)
2. [Add Catch Feature](5.2-catch/)
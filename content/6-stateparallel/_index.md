+++
title = "Parallel state processing"
date = 2021
weight = 6
chapter = false
pre = "<b>6. </b>"
+++


#### Parallel state processing ( Parallel state )

Now, we've done both steps of checking our data one after the other. But the address check does not depend on the results from checking the name of the registrant. So this is a great opportunity to speed things up and do two parallel data checks.

Step Functions allows us to create parallel state ( Parallel state ). Then the state machine executes many states in parallel. The parallel state causes the interpreter to execute multiple branches concurrently starting with the state named in the **StartAt** field and waiting until all branches have finished (reaching a complete state) before processing the **Next** field of the parallel state.

In this step we will:

+ Update state machine to run **Check Name** and **Check Address** in parallel using **Parallel** state.

+ Update state machine **Review Required?** to process results from parallel testing step. We need to do this step because **Parallel** state will return the results of each individual branch as elements in an array in the order defined by **Parallel** state.

#### Content
1. [Update workflow](6.1-updateworkflow/)
2. [Checkworkflow](6.2-checkworkflow/)
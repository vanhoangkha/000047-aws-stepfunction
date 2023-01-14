+++
title = "Service Orchestration"
date = 2021
weight = 1
chapter = false
pre = "<b>1.1 </b>"
+++


#### Service Orchestration ( Service Orchestration )

We use the term Orchestration to describe the act of coordinating distributed services through centralized workflow management, similar to how a conductor understands each part of an orchestra and directs each. Instruments work together to create their performance.

The main benefit of Orchestration to service orchestration is that it manages the logic centrally, rather than managing the distributed logic at the service level (each service itself decides what to do next). We can understand the entire process from start to finish through a single working interface.

This approach works well when services are highly coordinated, need to perform retry runs, handle specific errors, run tasks in parallel, or wait for several tasks to complete before continuing. subsequent tasks in the workflow (workflow).
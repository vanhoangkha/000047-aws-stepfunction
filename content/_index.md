+++
title = "Getting Started with AWS Step Functions"
date = 2021
weight = 1
chapter = false
+++

# Get started with AWS Step Functions

#### Overview

In this workshop, you will learn how to orchestrate workflows between services using a simple, yet powerful, fully managed AWS service, AWS Step Functions. We'll implement a pair of services as simple AWS Lambda functions and bundle them together into an example workflow involving parallel execution, branching logic, and pause/resume jobs.

#### Target

Through the workshop we will learn:

+ Benefits of implementing a service organization using the AWS Step Functions service.
Basics of using AWS Step Functions state machines:
  + Working with Lambda Functions using **Task** state.
  + Implement branching logic using **Choice** state.
  + Perform parallel work using **Parallel** state.
  + Implement **Pause/Resume** based on token & callback using **waitForTaskToken**.
Visualizing, debugging and auditing workflows using the AWS Step Functions console.

#### English version
[Intro to Service Coordination using AWS Step Functions](https://step-functions-workshop.go-aws.com/)

#### Content
1. [Introduction to the workshop](1-intro/)
2. [Prepare](2-prepare/)
3. [Service Dispatch ](3-serviceorchestration/)
4. [Improve Workflow](4-improveworkflow/)
5. [Error handling](5-errorhandling/)
6. [Parallel state processing](6-stateparallel/)
7. [Learn More](7-furtherlearning/)
8. [Resource Cleanup](8-cleanup/)
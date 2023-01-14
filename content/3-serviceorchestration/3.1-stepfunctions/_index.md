+++
title = "AWS Step Functions"
date = 2021
weight = 1
chapter = false
pre = "<b>3.1 </b>"
+++


#### AWS Step Functions

AWS Step Functions allows you to orchestrate services through a fully managed workflow so you can build and update applications quickly. A workflow is made up of a series of steps, with the output of one step serving as the input to the next.

Application development is simpler and more intuitive using Step Functions as it turns your workflow into a **state machine** diagram that is easy to understand, explain to others, and easy to change. You can track each step of the workflow execution as they happen (and afterwards), which helps you quickly identify problems and fix them. Step Functions automatically fires and tracks each step, and can retry steps in the event of an error, so your application workflow executes reliably and in the order you expect.

There is a lot to explore in the description above and at the end of this workshop you will see all the benefits. But the best way to understand a new service is to use it, so we will dive into Step Functions by starting with the basic concepts and steps we need to connect our services to together.

Step Functions works by representing our workflow as **state machine**. The concept of **state machine** is just a formalization of things that you already have a very clear intuitive understanding from writing any basic programming code.

**State machine** is a way of describing a set of workflow steps divided into named states. Each state machine has one startup state and always only one active state (during execution). The active state takes some input and usually performs some action using that input, which produces the output. **State machine** transitions from one state to the next based on their state and the explicit connections we allow between states.

Let's get hands-on so you can see how **Step Functions** works. We'll start by writing a Step Functions state machine to create a simplified model of the account registration workflow. With AWS Step Functions, there are several different types of states (or steps) that we can use to create our workflow state machine, and the simplest is the **Pass** State. The Pass state simply passes its input to its output, doing no work. Passing states are very useful when building and debugging state machines, so we'll use them in this step to start outlining our workflow.

#### Content
1. [Initialize workflow](3.1.1-workflow/)
2. [Check Workflow](3.1.2-tryworkflow/)
+++
title = "Dispatch Service"
date = 2021
weight = 3
chapter = false
pre = "<b>3. </b>"
+++


#### Dispatch Service

Coming back to our previous question of how do we orchestrate the services into the process.

At first glance, the simplest solution for you is to add logic to the **Account Application** service. Once the application is submitted, we can make two interservice calls to the **Data Checking** service to check the name and address. Then, once we have answers for both, we can flag the booking for review if either **Data Checking** is flagged, if not, we can approve the booking automatic. Certainly this approach will work, but there are some downsides that come later.

First, we'll need to handle the timeout and error conditions subtly. What if we don't get a prompt response from our interservice call, or if we get a temporary error and want to retry the request? If we're linking these services together directly in the service's logic, we'll need to add some fallback/retry logic. This isn't a big deal in essence, but it slows down our main thread while it's waiting to retry those requests.

Another benefit missed here is that if we just implement the process logic in the service itself, it won't be easy to create a visual representation of this workflow design or execution history. its. Visualizing the shape of a business process is of great value, as it helps non-technical users understand and validate the business logic we are using in our systems. Furthermore, this workflow starts out very simple, but as our workflow increases in complexity, it becomes more and more difficult to test the entire workflow execution and Debugging isn't easy either.

AWS has a simple yet incredibly powerful tool to help us streamline our workflows in a way that addresses all of these concerns: **AWS Step Functions**.

#### Content
1. [AWS Step Functions](3.1-stepfunctions/)
2. [Task State Type](3.2-taskstate/)
3. [Update Permissions](3.3-updatepermission/)
4. [State Input/Output](3.4-stateinputoutput/)
5. [Choice State Type](3.5-choicestatetype/)
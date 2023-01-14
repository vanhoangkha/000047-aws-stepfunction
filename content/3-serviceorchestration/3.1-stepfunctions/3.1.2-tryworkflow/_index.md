+++
title = "Check workflow"
date = 2021
weight = 2
chapter = false
pre = "<b>3.1.2 </b>"
+++


#### Check workflow

At this point, even though we've created a valid state machine, it doesn't do anything because the Pass state we're using in our definition just passes the input straight to the output of it without doing any work. Our state machine just transitions through the three Pass states and ends. However, let's quickly try the execution so we can see this.

1. Click **Start execution**.

![AWS Step Functions](/images/3.1.2/0001.png?featherlight=false&width=90pc)

2. Every time we ask Step Functions to execute a state machine, we can provide some initial input. Let's keep the original example input and click **Start execution**.

![AWS Step Functions](/images/3.1.2/0002.png?featherlight=false&width=90pc)

3. You will now see the details page of the execution we just enabled. Click on any step name in the visualization and note how we can see the input and output values ​​for each state during the execution.

![AWS Step Functions](/images/3.1.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.2/0005.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/3.1.2/0006.png?featherlight=false&width=90pc)

Now that you understand how to define and execute a state machine, let's update our state machine definition to do some work by calling the **Data Checking** service to check the name and address. For our new definition, we will use the **Task** state to do the work.
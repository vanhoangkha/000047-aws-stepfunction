+++
title = "Create Cloud9 Instance"
date = 2021
weight = 1
chapter = false
pre = "<b>2.1 </b>"
+++


#### Create Cloud9 Instance

AWS Cloud9 is an integrated development environment (IDE) that allows you to write, run, and debug your code in a browser. Cloud9 includes a code editor, debugger, and command line interface. Cloud9 comes pre-packaged with essential tools for popular programming languages, including JavaScript, Python, PHP, and more.

{{%notice tip%}}
During the steps in this workshop, use an IAM User with **Administrator Access** privileges instead of using the root account. \
This workshop will be conducted in Region Singapore.
{{%/notice%}}

1. Go to [Cloud9 service interface](https://ap-southeast-1.console.aws.amazon.com/cloud9/home/product)
 + Click **Create environment**.

![AWS Step Functions](/images/2.1/0001.png?featherlight=false&width=90pc)

2. Enter the name **StepFunctions**, then click **Next step**.

![AWS Step Functions](/images/2.1/0002.png?featherlight=false&width=90pc)

3. Keep the basic option, drag the screen to the bottom, and click **Next step**.

![AWS Step Functions](/images/2.1/0003.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/2.1/0004.png?featherlight=false&width=90pc)

4. Click **Create environment**, and wait a few minutes for the Cloud9 instance environment to be completely initialized.

![AWS Step Functions](/images/2.1/0005.png?featherlight=false&width=90pc)

5. Click the **X** icon to close unnecessary tabs.
  + Click the **+** icon, then click **New Terminal** to open a new command line interface.

![AWS Step Functions](/images/2.1/0006.png?featherlight=false&width=90pc)

The next step is to create sample services for our workflow.

![AWS Step Functions](/images/2.1/0007.png?featherlight=false&width=90pc)
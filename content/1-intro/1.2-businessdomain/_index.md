+++
title = "Business domain"
date = 2021
weight = 2
chapter = false
pre = "<b>1.2 </b>"
+++


#### Business domain ( Business domain )

It's easier to learn about orchestration services when we set up workflows based on a specific business area. In this workshop we will revolve around a number of services (operating as simple Lambda Functions), simulating a simplified process in the banking sector. The workshop will create a workflow to manage the new bank account creation process by checking some of the subscriber's information automatically as well as in conjunction with the controller.

Our sample system will include:
 + Service **Account Applications**: Process new applications. Request a reply from **Data Checking** service to provide verification of registrant's name and address information.
 + Service **Data Checking**: Check the name and address of the registrant.
 + **Accounts** service: responsible for opening and operating bank accounts, in this workshop we will not fully implement this service but focus on 2 services **Account Applications** and * *Data Checking**. **Accounts** service will be the destination when the application to open a new account is approved.

In the image below we can see the steps in the account registration process, and we will use a dispatch service to create a detailed workflow for these steps:

![StepFunctions](/images/SF/001.png?width=60pc)
+++
title = "Test Sample Service"
date = 2021
weight = 3
chapter = false
pre = "<b>2.3 </b>"
+++


#### Sample Service Test - Overview

In this section, we will manually interact with each Lambda function to understand our **Account Application** service and **Data Checking** service API.

These services are implemented as a collection of AWS Lambda functions. **Account Application** service provides us with the following features:

 + Submit a new registration form.
 + View a list of subscriptions by status.
 + Flag a subscription for review.
 + Approve or reject the application

**Data Checking** service allows us to:

 + Check a name to make sure it's valid.
 Validate an address that matches a specific rule.

{{%notice tip%}}
Although the **Account Application** service can flag a subscription, we have not yet created the logic to determine when a booking can be flagged. \
We are just setting up the basic capabilities to coordinate with the **Data Checking** service.
{{%/notice%}}

#### Content
1. [Check Service **Account Application**.](2.3.1-account/)
2. [Service Check **Data Checking**.](2.3.2-datacheck/)
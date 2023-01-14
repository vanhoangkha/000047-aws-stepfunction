+++
title = "Data Checking"
date = 2021
weight = 2
chapter = false
pre = "<b>2.3.2 </b>"
+++

#### Check **Data Checking** service.

1. Run the command below in the command line interface to check the name matches or not.
```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_NAME", "data": { "name": "Spock" } }' /dev/stdout
```
  + Returns ** {"flagged":false}** , so this name matches.

![AWS Step Functions](/images/2.3.2/0001.png?featherlight=false&width=90pc)

2. Run the command below, this time the name we will try to set does not match.

```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_NAME", "data": { "name": "evil Spock" } }' /dev/stdout
```
  + Returns **{"flagged":true}**, so this name is not suitable.

![AWS Step Functions](/images/2.3.2/0002.png?featherlight=false&width=90pc)

3. Run the following command in the command line interface to check if the address matches.
```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_ADDRESS", "data": { "address": "123 Street" } }' /dev/stdout
```
  + Returns ** {"flagged":false}** , so this address matches.

![AWS Step Functions](/images/2.3.2/0003.png?featherlight=false&width=90pc)

4. Run the command below, this time the name we will try to set does not match.

```
aws lambda invoke --function-name sfn-workshop-DataChecking --payload '{"command": "CHECK_ADDRESS", "data": { "address": "DoesntMatchAddressPattern" } }' /dev/stdout
```
  + Returns **{"flagged":true}**, so this address does not match.

![AWS Step Functions](/images/2.3.2/0004.png?featherlight=false&width=90pc)

As you can see, the **Data Checking** service just returns a plain JSON response with a variable, flagged to return true if the value needs rechecking by the controller.

We now have all the basic capabilities needed in our services to start connecting them together to implement the initial steps of application processing.

Our problem is "how to tie these services together to deploy into the workflow we want?"
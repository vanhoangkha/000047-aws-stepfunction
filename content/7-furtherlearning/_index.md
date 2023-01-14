+++
title = "Learn More"
date = 2021
weight = 7
chapter = false
pre = "<b>7. </b>"
+++


#### Learn more about AWS Step Functions

Step Functions can do more than orchestrate Lambda functions.

+ [**Activities**](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-activities.html) is a feature of AWS Step Functions that allows you to create a task in state machine whose back-end work is done by compute resources anywhere including Amazon Elastic Compute Cloud (Amazon EC2), Amazon Elastic Container Service (Amazon ECS), mobile devices, or anywhere that we can connect to AWS Step Functions API via HTTPS.

+ [**Map**](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-map-state.html) state can be used to run a set of steps for each element in the input array. While **Parallel** state executes multiple branches using the same input, **Map** state executes a set of steps multiple times for each element in the input array.

+ [**Wait**](https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-wait-state.html) state delays state machine from continuing to execute within a specified time. You can configure the time relative to the state , for example the time since the state started executing. In addition, you can also set a fixed time.

Step Functions can integrate directly with a wide variety of AWS services including AWS Batch, Amazon DynamoDB, Amazon ECS and Fargate, Amazon SQS and SNS, AWS Glue, Amazon Sage Maker... See [full list] (https://docs.aws.amazon.com/step-functions/latest/dg/concepts-service-integrations.html).


{{%notice tip%}}
If you want to dig deeper into Step Functions, the best step is through [**Developer Guide**](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html ). Then learn the [**project samples**](https://docs.aws.amazon.com/step-functions/latest/dg/create-sample-projects.html). Finally, there are webinars, blogs, reference architectures, videos and resources at [**AWS Step Functions Resources**](https://docs.aws.amazon.com/step-functions/latest/dg) /concepts-service-integrations.html).
{{%/notice%}}
+++
title = "Create sample services"
date = 2021
weight = 2
chapter = false
pre = "<b>2.2 </b>"
+++


#### Creating sample services - Overview

In this workshop, we will use the AWS Serverless Application Model (SAM) to write, deploy, and test functionality in our services. You do not need to have any experience with AWS SAM; The workshop will explain as we go on, so you can also learn how SAM helps you build a serverless system.

In this step we will create a Lambda function that, when called together, can be thought of as our **Account Application** service. Specifically, we will implement functions that will allow new registrations to be submitted (including a profile with name and address only), flag registrations for review, and mark registrations as approved or rejected. We'll also add the ability to list all subscriptions in a certain state (like DEPOSTED or FLASHED) to avoid the hassle of checking the service's datastore directly.

#### Install AWS SAM CLI
1. In the terminal interface of the Cloud9 instance, run the command below.
  + Set a new sudo password for the ec2-user of the Cloud9 instance, for example **123456**.

```
sudo passwd ec2-user
```

![AWS Step Functions](/images/2.2/0001.png?featherlight=false&width=90pc)

2. In the terminal interface of the Cloud9 instance, run the command below.

```
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
+ Enter [sudo] password for ec2-user: **123456**.
  + Press the **Enter** key.

![AWS Step Functions](/images/2.2/0002.png?featherlight=false&width=90pc) 

3. In the terminal interface of the Cloud9 instance, run the command below to configure the PATH for homebrew and clone the workshop's repo.
```
# Then get brew into our $PATH
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile

# Install the AWS SAM CLI
brew tap aws/tap
brew install aws-sam-cli

# Bootstrap our initial services into ./workshop-dir
git clone https://github.com/gabehollombe-aws/step-functions-workshop.git
cp -R step-functions-workshop/sam_template/ workshop-dir

# Change to our new directory
cd workshop-dir
```
  The installation process will take a few minutes.

![AWS Step Functions](/images/2.2/0004.png?featherlight=false&width=90pc)

![AWS Step Functions](/images/2.2/0005.png?featherlight=false&width=90pc)

4. Run the command below to build Lambda functions, ready for deployment.
```
Sam build
```

![AWS Step Functions](/images/2.2/0006.png?featherlight=false&width=90pc)

5. Run the command below to deploy the Lambda functions and DynamoDB, note the answer as below:
  + Information Stack Name [sam-app]: **step-functions-workshop**.
  + AWS Region information [ap-southeast-1]: **ap-southeast-1**.
  + Confirm changes before deploying [y/N]: FEMALE
  + Allow SAM CLI IAM role creation [Y/n]: Y
  + Save arguments to configuration file [Y/n]: Y
  + SAM configuration file [samconfig.toml]: samconfig.toml
  + SAM configuration environment [default]: Press Enter

```
# Run our first deploy in guided mode. Answer the interactive questions the same way that the comments show below:
sam deploy --guided
```

![AWS Step Functions](/images/2.2/0007.png?featherlight=false&width=90pc)


![AWS Step Functions](/images/2.2/0008.png?featherlight=false&width=90pc)

{{%notice warning%}}
If you get an error in the deploy step, try replacing step 6 with the steps in [Run sam deploy with self-created config file.](#run-sam-deploy-by-file-config-self-created)
{{%/notice%}}

6. Spend some time checking the contents of the files we have created in the **workshop-dir** directory:

```bash
functions/account-applications/submit.js
# Lambda SubmitApplication function: find, flag, reject and approve registrations.

functions/account-applications/AccountApplications.js
# This is a generic file that provides CRUD operations for the **Account Application** data type. Used by other functions.

functions/data-checking/data-checking.js
# This file implements **DataChecking** Lambda function, its job is to handle name and address checking requests with some simple rules.

template.yaml
# This is the AWS SAM file that will declare the resources that we want to create and deploy in our solution, with a structure quite similar to the CloudFormation template because SAM is built on top of CloudFormation.
```
The next step is to test the sample services for our workflow.

#### Run sam deploy with self-created config file.

{{%notice warning%}}
Only do this step if sam deploy -guided fails.
{{%/notice%}}

1. Check the directory path is in workshop-dir
2. Run the command below to create the SAM CLI bucket (you can replace yourname = your name)
```
aws s3 mb s3://stepfunctions-workshop-yourname --region ap-southeast-1
```

![AWS Step Functions](/images/2.2/0009.png?featherlight=false&width=90pc)

3. Run the command below to create the file samconfig.toml

```
touch samconfig.toml
```
![AWS Step Functions](/images/2.2/00010.png?featherlight=false&width=90pc)

4. Double click on the samconfig.toml file and add configuration content as follows:
```
version=0.1
[default.global.parameters]
stack_name = "step-functions-workshop"

[default.deploy.parameters]
stack_name = "step-functions-workshop"
s3_bucket = "stepfunctions-workshop-yourname"
s3_prefix = "stepfunctions"
region = "ap-southeast-1"
confirm_changeset = false
capabilities = "CAPABILITY_IAM"
```
  + Press Ctrl + S to save the file samconfig.toml.

![AWS Step Functions](/images/2.2/00011.png?featherlight=false&width=90pc)

5. Run the command below to execute the redeployment.
```
sam deploy
```
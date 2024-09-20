# Building an application in IAM Identity Center

!!! info "Working in progress"

## Introduction
### Background
Being working with AWS for so long, its [pillars of well-architect](https://aws.amazon.com/blogs/apn/the-6-pillars-of-the-aws-well-architected-framework/) imprinted into my brain. 

One of it is cost optimization. 

Even though I have automation to turn on/off for our services outside business hours. 
Some of our project is going to maintenance phase. Features need to be tested in development and staging environments are not as many anymore. 

Our systems have too much un-use time.

If only we have a button that can turn on/off infrastructure whenever we like.

If only we have a function so that even developer can turn on/off system periodically whenever they like..

And of course accessing to the system is forbidden to unauthenticated user.

### IAM Identity Center
Beside being a place to centralize your AWS Accounts' access. IAM Identity Center have option for your customize app. 

Since only IAM Identity Center authenticated users can access our app, we don't have to worry that the site is public accessible.

So why don't we leverage it and let's create a web app that has bunch of buttons for manual on/off infrastructures.

## System Implementation
### Design
![AWSDiagram](iamic-app-design.png)
There will be 2 major parts. One is our application backend and frontend reside in a centralized AWS account. 

Other will be process to turn on/off resources in registered satellite accounts. Which will be different for each, could be lambda function calling AWS API to turn on/off function. Or in my case, since I created our resources with CDK we can easily boot up a Codebuild Instance and changing infra with a simple command. (1)
{ .annotate }

1.  And if setting up turn on/off process is the same for every satellite accounts. You can create Service Catalog and share to the whole Organization for simplicity.

Let's just talk about our application in this post. 

Of course my lazy ass will try to avoid backend code as much as possible. So I will be using [AWS Pre-config Authentication App](https://console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:us-east-1:520945424137:applications/cloudfront-authorization-at-edge).

!!! warning annotate "AWS Pre-config App parameters"
    Remember to include your backend URL to Content-Security-Policy in **HttpHeaders** parameter!
    I also recommend that you prepared your frontend bucket and Cloudfront OAI to access it so you can put their information in the app' parameters.

    To sum up, you should put following parameters and leave others as is: <br> **HttpHeaders**, **OriginAccessIdentity**, **SignOutUrl** and **S3OriginDomainName**.

The app will cloudfront distribution, user pool and lambda function that handles authentication process for us. 

The app resides in us-east-1. Change to your region before creating the app.

<h4> Other components: </h4>
`IAM Identity Center`
:   Our IDP and where we will setup our custom app.

`Frontend bucket`
:   Serve React's SPA statically.

`Backend API Gateway`
:   API front for our backend function, user must authenticated with Cognito, receive token and using that when requesting to our API.

`Backend Function`
:   Handle request accordingly. For now I only have 2 functions, read all infra status and change status to on/off of 1 infra environment.

`Status DynamoDB`
:   Database storing status of each infra environment.

`DynamoDB stream`
:   Streaming whenever there is a change in one of our records. Trigger data processing lambda function.

`Event Publisher Function`
:   Receive data from dynamoDB stream and publish a SNS topic with attribute

`Change SNS Topic`
:   Send message to all subscribers in all accounts. Each subscription will have a filtering to check if the message destined to its account then process with appropriate action..

### Implement

### Running cost
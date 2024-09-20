# Building an application in IAM Identity Center

!!! info "Working in progress"

## Introduction
### Background
Being working with AWS for so long, its [pillars of well-architect](https://aws.amazon.com/blogs/apn/the-6-pillars-of-the-aws-well-architected-framework/) imprinted into my brain. 

One of it is cost optimization. 

Even though I have automation to turn on/off for our services outside business hours. 
Some of our project is going to maintenance phase. Features need to test in dev and stg environments are not as much as before. 

So automatically on at daylight is not even feasible anymore.

If only we have a button that can turn on/off infrastructure when we like...

### IAM Identity Center
Beside being a place to centralize your AWS Accounts' access. IAM Identity Center have option for your customize app. 

Since users can only access our app via IAM Identity Center portal, we don't have to worry that the site is public accessible.

So why don't we leverage it and let's create a web app that has bunch of buttons for manual on/off infrastructures.

### System Design
![AWSDiagram](iamic-app-design.png)

!!! warning inline end "AWS Pre-config App parameters"
    
    Remember to include your backend URL to Content-Security-Policy in *HttpHeaders* parameter!
    It's also recommend that you prepared your frontend bucket and Cloudfront OAI to access it so you can put their information in app's parameter.
    To sum up, you should put following parameters: HttpHeaders, OriginAccessIdentity, SignOutUrl and S3OriginDomainName

Of course my lazy ass will try to avoid backend code as much as possible. So I will be using [AWS Pre-config Authentication App](https://console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:us-east-1:520945424137:applications/cloudfront-authorization-at-edge).

The app will cloudfront distribution, user pool and authentication process for us. 

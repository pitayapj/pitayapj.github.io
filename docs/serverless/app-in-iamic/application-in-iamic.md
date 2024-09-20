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
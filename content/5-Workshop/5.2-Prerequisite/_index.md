---
title: "Prerequisites"
date: 2026-11-18
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Prerequisites

Prepare the items below before you start. The whole workshop runs in one region: **US East (N. Virginia), `us-east-1`**.

### 1. AWS account and permissions

- An AWS account with enough credit. The estimated cost of this workshop is about **$8.05** (see section 5.7), so keep at least $20 of credit free.
- An **IAM user** to sign in. Do not use the root user. In a dedicated lab account, attaching the `AdministratorAccess` policy to this user is the simplest way. The user must be able to create CloudFormation stacks, IAM roles, EC2 instances, Lambda functions, DynamoDB tables, SNS topics, Parameter Store parameters, EventBridge Scheduler schedules, Budgets and CloudWatch alarms.
- **MFA** turned on for the root user and for the IAM user.

### 2. Network

- A **default VPC** in `us-east-1`. Open **VPC → Your VPCs** and check that one VPC is marked as default.
- If there is no default VPC, choose **Actions → Create default VPC**. Another option is to launch the lab instance yourself and pass its ID to the template (see 5.3).

![The default VPC in us-east-1 (Resource map)](../../images/5-Workshop/5.2-prerequisite/default-vpc.webp)

### 3. Tools

- A web browser for the AWS Management Console.
- A mailbox that you can open, because Cost Guard sends its alerts by email and SNS needs you to confirm the address.
- Optional: AWS CLI v2, configured with `aws configure`, if you prefer the command line to the console.

### 4. Files

- The CloudFormation template [cost-guard.yaml](../../attachments/cost-guard.yaml). It creates the whole system in one stack.

### 5. Know before you start

- **The lab instance starts as soon as the stack is created.** A Windows t3.small costs $0.0392 per hour in `us-east-1`, so test soon after the deployment and do not leave it running. On a Free Plan account only t3.micro, t3.small, t8i.micro and t8i.small can be launched.
- **Plan about 45 minutes** for the deployment and the three layer tests, not counting the wait for the daily schedule.
- Cost Guard **never starts the instance by itself**. You start it manually when you need it.

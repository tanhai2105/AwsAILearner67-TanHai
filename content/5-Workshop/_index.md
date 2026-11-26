---
title: "Workshop"
date: 2026-11-18
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Cost Guard: Protecting Windows Lab Credits on AWS with Three Serverless Layers

#### Overview

A Windows EC2 instance is billed by the hour. A forgotten machine can use up a small credit budget in days: a t3.small left running 24/7 costs about $31.04 per month, more than the $20 credit budget of this project. AWS Budgets can warn about cost, but its billing data is delayed, so it cannot stop the machine in time on its own.

In this workshop you build **Cost Guard**, a serverless system that stops the lab instance automatically. It uses three independent layers, and each layer covers the weakness of the others:

- **Layer 1 – Schedule Guard:** stops the instance every day at 22:00 (Asia/Ho_Chi_Minh), whether or not anyone remembers.
- **Layer 2 – Cost Meter:** runs every 5 minutes, estimates the monthly cost from the running time and the hourly price, and stops the instance when the estimate reaches the threshold of about $14.
- **Layer 3 – Budget Backstop:** an AWS Budget follows the real bill, and at 80% and 100% of $20 it triggers a Lambda function that stops the instance.

You deploy the whole system with one CloudFormation template, test each layer on purpose, review security and cost, and delete everything at the end so that no charge is left behind.

#### AWS Services Used

- **Amazon EC2 and Amazon EBS:** the Windows lab machine that Cost Guard protects.
- **Amazon EventBridge Scheduler:** the daily stop and the 5-minute run of the cost meter.
- **AWS Lambda:** the logic of the three layers.
- **Amazon DynamoDB:** the record of every action and cost estimate.
- **Amazon SNS:** email alerts and the link between Budgets and Lambda.
- **AWS Budgets:** the monthly budget and its 80% and 100% alerts.
- **AWS Systems Manager Parameter Store:** price, threshold and budget limit.
- **Amazon CloudWatch:** logs and alarms on function errors.
- **AWS IAM:** one least-privilege role for each function.
- **AWS CloudFormation:** the whole system in one stack.
- **AWS Cost Explorer:** the comparison of real cost and estimate after cleanup.

#### Content

1. [Introduction](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisite/)
3. [Deploy the Foundation](5.3-Foundation/)
4. [Layer 1 – Schedule Guard](5.4-Layer1-Schedule/)
5. [Layer 2 – Cost Meter](5.5-Layer2-CostMeter/)
6. [Layer 3 – Budget Backstop and Monitoring](5.6-Layer3-Backstop/)
7. [Security and Cost Review](5.7-Security/)
8. [Clean Up](5.8-Cleanup/)

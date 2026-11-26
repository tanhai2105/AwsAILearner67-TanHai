---
title: "Introduction"
date: 2026-11-18
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Cost Guard

- A Windows EC2 instance is billed by the hour. A t3.small left running 24/7 costs about $31.04 per month, more than the $20 credit budget of this project.
- AWS Budgets can warn about cost, but its billing data refreshes only a few times per day, so it cannot stop a forgotten machine in time on its own. Cost Guard stops the machine automatically with three serverless layers.

#### Workshop overview

In this workshop, you will deploy Cost Guard with one CloudFormation template. The three layers work independently, and each one covers the weakness of the others.

- **Layer 1 – Schedule Guard:** EventBridge Scheduler and a Lambda function stop the instance every day at 22:00 (Asia/Ho_Chi_Minh), whether or not anyone remembers.
- **Layer 2 – Cost Meter:** a Lambda function runs every 5 minutes, estimates the monthly cost from the running time and the hourly price, and stops the instance when the estimate reaches about $14.
- **Layer 3 – Budget Backstop:** an AWS Budget follows the real bill, and at 80% and 100% of $20 it triggers a Lambda function that stops the instance.

![overview](../../images/5-Workshop/5.1-Workshop-overview/diagram1.png)

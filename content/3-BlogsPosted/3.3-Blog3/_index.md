---
title: "Blog 3"
date: 2026-11-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

### COST GUARD: THREE SERVERLESS LAYERS THAT STOP A WINDOWS INSTANCE TO PROTECT AWS FREE PLAN CREDIT

**Published on:** AWS Study Group (Facebook)

My team (AwsAILearner67, FCAJ) runs labs in one shared AWS Free Plan account with limited credit. The most common problem is not technical but human: people finish studying and forget to stop the instance. A Windows `t3.small` running 24/7 costs about **$31.04 a month** (EC2 $28.62 + EBS $2.40), more than the whole project's $20 budget.

AWS Budgets can send alert emails, but it has two limits: billing data only updates a few times a day, and Budgets does not stop instances. So we built **Cost Guard**: three independent layers watching one instance, so if one layer fails the other two are still there.

![Cost Guard architecture: EventBridge Scheduler, three Lambda functions, AWS Budgets, SNS, DynamoDB, Parameter Store and a Windows EC2 instance in us-east-1](../../images/3-BlogsPosted/3.3-Blog3/costguard-architecture.png)

**The three layers:**

- **Layer 1 – Scheduled stop:** EventBridge Scheduler calls the Lambda `CostGuard-ScheduleHandler` at 22:00 every day in the `Asia/Ho_Chi_Minh` time zone. If the instance is running, the function calls `StopInstances`; if it is already stopped, it only records `schedule_noop`. This layer needs no billing data, so it is the most reliable. We deliberately do not start the instance automatically: whoever needs it starts it, so nothing is billed while nobody is using it.
- **Layer 2 – Cost meter (the part we built ourselves):** every 5 minutes, the Lambda `CostGuard-CostMeter` checks the instance. If it is running, the function adds the cost of those 5 minutes ($0.0392/hour × 5/60 ≈ $0.0033) to the month's total and stores a record in DynamoDB. When the total reaches the $14 threshold, it stops the instance and sends an email through SNS. It does not wait for billing, so it reacts within minutes.
- **Layer 3 – Last safety net:** an AWS Budget of $20 alerts at 80% and 100% of actual spend. Budgets publishes to the SNS topic `costguard-budgets`, which calls the Lambda `CostGuard-BudgetBackstop` to stop the instance and send a confirmation email. Billing is slow, so this layer is the last resort.

On top of that, a CloudWatch alarm watches the errors of all three Lambda functions and emails through a separate topic, `costguard-errors`. If a layer breaks, the team knows right away instead of finding out when the credit is gone.

**Why the threshold is $14:** Layer 2's threshold is about 70% of the budget. At $0.0392/hour, $14 is about 357 running hours, far more than the planned 120 lab hours a month. The remaining $6 covers the billing delay of Layer 3. The price, the threshold and the instance ID live in Parameter Store instead of being hard-coded, so using it for another instance only means changing the values.

**Real test results (us-east-1, 22 and 25/11/2026):**

| Layer | Test | Result |
|---|---|---|
| 1 | Invoke the function directly while the instance is running | Instance stopped after 3 seconds |
| 2 | Lower the threshold to `0.001` and wait for the next run | CostMeter stopped the instance 3 min 47 s after the threshold was saved (from CloudTrail) |
| 3 | Publish a test message to the `costguard-budgets` topic | Instance stopped after 7 seconds |

All three runs left a record in DynamoDB (`schedule_stop`, `auto_stop`, `budget_stop`) and an email in the team inbox. Remember to set the threshold back to 14 after testing.

**Cost:** according to the AWS Pricing Calculator, for 120 lab hours a month: EC2 $4.70 + EBS $2.40 + Lambda $0.02 = **$7.12/month**. With the public IPv4 address, CloudWatch and other services it is about $8.05. Compared with $31.04 for an instance left running, Cost Guard saves about $23.92 a month (~77%). The "protection" part (Lambda, on-demand DynamoDB, SNS, Scheduler, Parameter Store) costs almost nothing.

**Conclusion and thoughts:**

- Cost protection should have several layers: one based on time, one on an estimate, one on the real bill. Each covers the weakness of the others.
- Each Lambda function has its own IAM role, and `StopInstances` is allowed only on the lab instance. A misconfiguration cannot stop someone else's instance.
- The lab instance opens no inbound port and requires IMDSv2.
- Everything lives in one CloudFormation template, so deleting the stack removes almost everything. In our real cleanup we still had to delete two things created outside the stack by hand: the IAM role that the Scheduler console created, and the `cf-templates` bucket holding the uploaded template.

Full workshop (step by step, screenshots, template): [nguyenducbinh-aws.github.io/AwsAILearner67](https://nguyenducbinh-aws.github.io/AwsAILearner67/)

**References:**

- [EventBridge Scheduler: schedule types and time zones](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)
- [AWS Pricing Calculator](https://calculator.aws/)

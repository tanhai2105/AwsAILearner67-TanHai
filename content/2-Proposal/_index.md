---
title: "Proposal"
date: 2026-11-18
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cost Guard for Windows Labs on AWS
## A Serverless Three-Layer Credit Protection Solution

### 1. Executive Summary

Cost Guard is designed for students and small teams who practice on AWS with a Free Plan account, to stop a Windows lab machine from using up the account credit. It protects one Windows EC2 instance (t3.small) and keeps the total project spend under a $20 budget. The system runs on AWS serverless services in US East (N. Virginia) and uses three protection layers: a scheduled stop with EventBridge Scheduler and Lambda, a near-real-time cost meter with Lambda, DynamoDB and Parameter Store, and a backstop with AWS Budgets and SNS. Alerts are sent by email, and all supporting services fit within the AWS Free Tier; the Windows EC2 instance is the only meaningful cost.

### 2. Problem Statement

### What's the Problem?

AWS Free Plan credits are limited, and a Windows EC2 instance is billed by the hour for as long as it runs. A t3.small left running 24/7 costs about $31.04 per month, which is more than a $20 credit budget, and a machine forgotten after a lab session keeps spending credit. AWS Budgets can send cost alerts, but its billing data refreshes only a few times per day and it cannot stop the machine, so there is no real-time safeguard.

### The Solution

Cost Guard uses three complementary layers:

- **Layer 1 — Schedule Guard:** An EventBridge Scheduler rule stops the instance outside lab hours. It does not depend on billing data, so it is the most reliable layer. The schedule only stops the machine; starting it stays a manual action so that no credit is spent while nobody is using it.
- **Layer 2 — Cost Meter (original contribution):** A Lambda function runs every 5 minutes. It estimates the cost of the running instance from its running time and a configured hourly price, and adds it to the estimated cost of earlier sessions in the current month. When the total crosses an internal threshold, the instance is stopped and an alert is sent through SNS.
- **Layer 3 — Budget Backstop:** AWS Budgets monitors the actual account spend. As spend approaches the limit, it triggers a Lambda function through SNS to stop the instance as the final safety net.

### Benefits and Return on Investment

Under the planned usage of 120 lab hours per month, the estimated cost is about $7.12 per month. The same instance left running 24/7 would cost $31.04 per month. Cost Guard therefore avoids roughly $23.92 per month, about 77%, and keeps the project under the $20 budget. Layer 2 gives cost awareness within minutes of the machine starting, without waiting for billing data. The hourly price and the cost threshold are stored in Parameter Store instead of being hardcoded, so the system can be reused for other instance types or accounts.

### Goals and Scope

Cost Guard is built for students and small teams that run one Windows lab instance on an AWS account with limited credit.

Goals:

- Stop the instance automatically when lab hours end.
- Stop the instance when the estimated monthly cost reaches the internal threshold (about $14).
- Stop the instance as a last resort when the actual spend reaches 80% and 100% of the $20 budget.
- Keep the total project spend under $20.

Success criteria:

- Layer 1: the instance is stopped at 22:00, and the Lambda log and the DynamoDB record show it.
- Layer 2: with simulated cost data, the instance is stopped within 5 minutes after the threshold is reached, and an alert email arrives.
- Layer 3: a test notification on the budget topic stops the instance and sends a confirmation email.
- Cost Explorer after cleanup shows a total spend under $20, and no instance or volume is left behind.

Out of scope: managing more than one instance or account, starting the instance automatically, a web dashboard, and cost control for services other than the lab instance.

### 3. Solution Architecture

The system runs in a single AWS Region (US East, N. Virginia) and uses only serverless or managed services. EventBridge drives the schedule and the periodic cost check. Lambda functions handle stop logic and cost estimation. DynamoDB records session activity. SNS delivers notifications. Parameter Store holds configuration. Each Lambda function has its own least-privilege IAM role.

![Architecture Diagram](../images/2-Proposal/architecture.png)

### AWS Services Used

- **Amazon EC2 (Windows):** The lab machine being protected (t3.small, Windows Server). A Free Plan account can only choose t3.micro, t3.small, t8i.micro and t8i.small; the team chose t3.small (2 vCPU, 2 GiB), the larger of the two t3 sizes, because 1 GiB is too little for Windows Server.
- **Amazon EBS (gp3, 30 GB):** The root volume of the lab machine. It is billed for the whole month, even while the instance is stopped.
- **Amazon EventBridge:** EventBridge Scheduler triggers the scheduled stop and runs the cost meter every 5 minutes. Both use schedules.
- **AWS Lambda:** Runs the logic of each protection layer.
- **Amazon DynamoDB:** Stores session activity and cost estimates.
- **Amazon SNS:** Sends email alerts when a threshold is crossed or an error occurs.
- **AWS Budgets:** Monitors actual spend and triggers the final protection layer.
- **AWS Systems Manager Parameter Store:** Stores the hourly price, the cost threshold and the budget limit.
- **Amazon CloudWatch:** Collects Lambda logs and raises alarms on function errors.
- **AWS IAM:** Provides a least-privilege execution role for each Lambda function.
- **AWS CloudFormation:** Packages the stack for repeatable deployment and clean teardown.
- **AWS Cost Explorer:** Compares the estimated cost with the actual cost at the end of the project.

### Component Design

- **Scheduler Layer:** EventBridge Scheduler → ScheduleHandler Lambda → stops the EC2 instance on a fixed schedule and writes the event to DynamoDB.
- **Cost Meter:** EventBridge Scheduler (every 5 minutes) → CostMeter Lambda → reads price and threshold from Parameter Store → calculates the estimated cost from session records in DynamoDB → if the threshold is exceeded, stops the instance and publishes an SNS alert. The threshold applies to the EC2 running cost only; the EBS volume is accounted for separately in the budget.
- **Budget Backstop:** AWS Budgets alarm → SNS topic → BudgetBackstop Lambda → stops the instance → sends a confirmation email.
- **Notifications:** One SNS topic for cost alerts to the user; one SNS topic for operational errors.
- **Data:** A DynamoDB table that logs instance start and stop events and the periodic cost estimate of each session.
- **Security:** Each Lambda function has its own IAM role, and its stop permission is limited to the target instance.

### 4. Technical Implementation

**Implementation Phases**

1. *Research and design the architecture*: Study the services used by the project and design the three protection layers (Month 1–2).
2. *Estimate the cost and check feasibility*: Use the AWS Pricing Calculator to estimate the Windows instance and the supporting services, and check that the total stays under $20 (Month 2).
3. *Adjust the design to optimize cost and logic*: Settle the Layer 2 cost rule, the threshold and the schedule, and keep the running hours to the plan of 120 hours per month (Month 2–3).
4. *Build, test and validate*: Create the foundation (IAM roles, SNS topics, DynamoDB table, Parameter Store entries, AWS Budget), then deploy and test each layer: the scheduled stop, the threshold with simulated cost data, and the Budgets backstop with the CloudWatch alarms. Finish with an end-to-end validation and a comparison of the estimated and the actual cost in Cost Explorer (Month 3).

**Technical Requirements**

- AWS account and access: An AWS account with Free Plan credits, an IAM user (not the root user) and the US East (N. Virginia) Region. One email address receives the cost and error alerts.
- Protected resource: One Windows lab instance (t3.small) with a 30 GB gp3 root volume, planned for about 120 running hours per month.
- Three protection layers: A scheduled stop (EventBridge Scheduler and Lambda), a cost meter every 5 minutes (EventBridge Scheduler, Lambda, DynamoDB and Parameter Store) and a budget backstop (AWS Budgets, SNS and Lambda), each Lambda function with its own least-privilege IAM role.
- Monitoring and deployment: CloudWatch logs and alarms on function errors, SNS emails, and CloudFormation for repeatable deployment and clean teardown. Cost Explorer is used at the end to compare the estimated and the actual cost.

### 5. Timeline & Milestones

**Project Timeline**

- **Month 1 (07/09 – 06/10/2026):** Join the FCAJ program, read the internship rules and prepare the working environment. Then learn the AWS foundations: account setup, MFA, IAM, Budgets, Support, Console and CLI.
- **Month 2 (07/10 – 06/11/2026):** Study the services used by the project (Lambda, EventBridge, SNS, DynamoDB, Parameter Store, CloudWatch) and design the three layers.
- **Month 3 (07/11 – 14/12/2026):** Build, test and validate the solution.
  - Week 1: Foundation and Layer 1, then test that the scheduled stop works at the set hour.
  - Week 2: Layer 2, then test the threshold with simulated cost data and check that the instance stops within 5 minutes.
  - Week 3: Layer 3 and the CloudWatch alarms, then test the budget notification and the error alarms.
  - Week 4: End-to-end validation of the three layers, comparison of the estimated and the actual cost in Cost Explorer, and a check that no resource is left running after cleanup.
- **Post-deployment:** Keep the solution as a reusable template; extend it to other instance types or accounts by changing the Parameter Store values.

### 6. Budget Estimation

The estimate was built with the AWS Pricing Calculator for US East (N. Virginia), using on-demand pricing and a planned usage of 120 lab hours per month (4 hours per day for 30 days).

AWS Pricing Calculator estimate: [share link](https://calculator.aws/#/estimate?id=641acc2f03b075415ec10d1ab0de1590ab659dd9) and [PDF export](../attachments/costguard-pricing-estimate.pdf).

### Infrastructure Costs

- Amazon EC2 Windows (t3.small): 120 hours × $0.0392 per hour = $4.70 per month. In the Calculator the usage is entered directly as 120 Hours / Month, and the volume is priced in a separate EBS entry.
- Amazon EBS (gp3, 30 GB): $2.40 per month, billed for the whole month even when the instance is stopped.
- Public IPv4 address: $0.005 per hour for an in-use address, so 120 hours × $0.005 = $0.60 per month (not part of the Calculator estimate).
- AWS Lambda: about $0.02 per month (8,700 requests and 1,087.5 GB-seconds at 128 MB and 1 second per run), calculated without the Free Tier.
- Amazon DynamoDB (on-demand): about $0.002 per month (2,000 writes, 7,500 read request units and 0.001 GB of storage), left out of the Calculator because it is so small.
- Amazon CloudWatch: 3 standard alarms × $0.10 = $0.30 per month, plus about 0.01 GB of Lambda logs at $0.50 per GB ingested (about $0.005), calculated without the Free Tier.
- Parameter Store (standard parameters) and AWS Budgets (a tracking budget): $0.00 per month.
- EventBridge Scheduler is charged per invocation, and the first 14 million invocations per month are free. The project uses about 8,700 per month (the 5-minute schedule plus one daily stop), which is about $0.009 per month if the Free Tier is not counted.
- Amazon SNS (US East, standard topics): the first 1 million API requests and the first 1,000 email notifications per month are free. The project uses about 100 of each, so the cost is $0, and under $0.01 per month if the Free Tier is not counted. Delivery to Lambda has no per-message fee.

**Total:** $7.12 per month for the services in the Calculator (EC2, EBS and Lambda), and about $8.05 when DynamoDB, the public IPv4 address, CloudWatch, EventBridge Scheduler and SNS are added, all calculated without the Free Tier. Both are under the $20 budget.

For comparison, the same instance running 24/7 (730 hours) costs $31.04 per month: $28.62 for EC2, $2.40 for EBS and about $0.02 for Lambda and DynamoDB.

The internal threshold of Layer 2 is set at about 70% of the budget ($14). At $0.0392 per hour this equals about 357 running hours, well above the planned 120 hours. The gap leaves room for the billing delay of Layer 3.

### Cost Optimization

- Limit the running hours: the scheduled stop and the manual start keep the machine at about 120 hours per month instead of 730.
- Use services that charge per use and stay within the Free Tier: Lambda, DynamoDB on-demand, SNS and EventBridge.
- Use Parameter Store standard parameters, which have no charge.
- Use a 30 GB gp3 volume (the default size of the Windows image) with no extra IOPS or throughput.
- Compare instance types in the Pricing Calculator before launch and keep the smallest type that runs the lab comfortably.
- Avoid an Elastic IP, which is billed while allocated.
- Delete every resource after the demo and confirm it in Cost Explorer.

### 7. Risk Assessment

#### Risk Matrix

- Running out of credit midway because a machine is left running or a layer fails: high impact, medium probability.
- Three people sharing one AWS account, so one person can overwrite the settings of another or stop a machine that a teammate is using (for example, lowering the threshold for a test): medium impact, medium probability.
- A schedule set with the wrong time zone, so the stop runs at the wrong hour. The cron expression of an EventBridge rule is read in UTC, so it is easy to be several hours off: medium impact, medium probability.
- A schedule that never triggers the Lambda function, because the cron expression is written wrongly or the function has no permission for EventBridge to invoke it: medium impact, medium probability.
- Budgets data arriving late or being misread, because billing data refreshes only a few times per day: high impact, high probability (by design).
- Layer 2 estimate diverging from the actual bill: medium impact, medium probability.
- A Lambda function stopping the wrong instance because of a misconfigured IAM role: high impact, low probability.

#### Mitigation Strategies

- Credit running out: Layer 1 stops the instance every day at a fixed hour, and Layer 2 and Layer 3 stop it when the cost rises. One failing layer does not remove the other two.
- Shared account: each person uses their own IAM user, only one person launches the instance, and any change to the threshold, the price or the schedule is announced to the team and put back after the test.
- Time zone: use EventBridge Scheduler for the daily stop, because it lets the time zone be set explicitly (Asia/Ho_Chi_Minh) instead of relying on UTC. Test it once with a stop time a few minutes ahead before relying on it.
- Schedule not triggering: check the cron expression in the schedule preview, and confirm that the Lambda function allows EventBridge to invoke it (its resource-based policy). Look at the invocation count and the log after the first test.
- Budgets delay: treat Layer 3 only as the last safety net, and rely on Layer 2 for fast reaction.
- Estimate drift: set the threshold below the budget and compare the estimate with Cost Explorer at the end of the project.
- Wrong instance: each Lambda role can stop only the target instance.

#### Contingency Plans

- If a Lambda layer fails, stop the instance manually from the AWS Console and disable the EventBridge schedules.
- If the credit runs out during the project, terminate the Windows instance immediately.
- If the $20 budget is not enough, reduce the daily lab hours or switch to a smaller instance type and update the estimate.

### 8. Expected Outcomes

#### Technical Improvements

- **Reaction time:** From the moment the estimated cost reaches the threshold to the moment the instance is stopped is under 5 minutes, which is one Cost Meter interval.
- **Correct stops:** In the tests, each layer stops the instance every time it should, and never stops it when the cost is below the threshold. The workshop records the number of test runs and their results.
- **Spend:** The total project spend stays under $20, confirmed by Cost Explorer after cleanup.
- **Observability:** All three layers can be followed through Lambda logs, SNS emails and the DynamoDB session record.

#### Long-term Value

Because the instance ID, hourly price and threshold are stored in Parameter Store, the same system can protect another Windows lab account by changing those values, without changing code.

---
title: "Security and Cost Review"
date: 2026-11-18
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

## Security and Cost Review

This step reviews how Cost Guard handles security, what it costs and how to keep the cost low.

### 1. Security

**One IAM role for each function.** The three Lambda functions have separate roles, so a mistake in one function cannot use the permissions of another. The permission to stop an instance is limited to the **ARN of the protected instance**:

```json
{
  "Effect": "Allow",
  "Action": "ec2:StopInstances",
  "Resource": "arn:aws:ec2:us-east-1:<account-id>:instance/<instance-id>"
}
```

`ec2:DescribeInstances` is allowed on all resources, because this action does not support resource-level restriction. The other permissions are limited to the parameters, the table and the topics that the function uses.

**No credentials in code.** The functions use their execution roles. The Parameter Store values are only the hourly price, the threshold, the budget limit and the instance ID.

**Services call each other with their own roles.** EventBridge Scheduler uses a role that can invoke only the two functions. The topic policy of `costguard-budgets` lets only the Budgets service in your own account publish.

**No open ports.** The lab instance in this workshop uses its own security group, `costguard-lab-no-inbound`, which has no inbound rule at all, and it requires IMDSv2. If you let the template create the instance instead, it uses the default security group of the default VPC, which only allows traffic from members of the same group.

![Security group costguard-lab-no-inbound has no inbound rule](../../images/5-Workshop/5.7-security/67-sg-no-inbound.png)

**Check:** open **EC2 → Security Groups → `costguard-lab-no-inbound`**. Inbound rules count is **0 Permission entries** and the **Inbound rules** tab says "No security group rules found".

**To check:** open **IAM → Roles**, choose each `CostGuard` role and read its policy. The only action that touches EC2 state is `StopInstances` on one instance.

![Policy of the CostGuard-ScheduleHandlerRole role](../../images/5-Workshop/5.7-security/68-iam-schedulehandler-policy.png)

**Check:** `ec2:StopInstances` has the ARN of exactly one instance as its Resource, `ec2:DescribeInstances` is `*`, and `ssm:GetParameter` is limited to `/costguard/instance-id`. Read the other two roles the same way.

### 2. Cost estimation

The estimate was made with the AWS Pricing Calculator for US East (N. Virginia), using on-demand prices and 120 lab hours per month. Prices are without the Free Tier.

- Amazon EC2 Windows (t3.small): 120 hours × $0.0392 = **$4.70**
- Amazon EBS (gp3, 30 GB): **$2.40**, billed for the whole month even while the instance is stopped
- Public IPv4 address: 120 hours × $0.005 = **$0.60**
- AWS Lambda: about **$0.02**
- Amazon DynamoDB (on-demand): about **$0.002**
- Amazon CloudWatch (3 alarms and logs): about **$0.31**
- EventBridge Scheduler and Amazon SNS: under **$0.01** each

The services in the Calculator (EC2, EBS and Lambda) add up to **$7.12 per month** ([estimate](https://calculator.aws/#/estimate?id=641acc2f03b075415ec10d1ab0de1590ab659dd9)). With the other services added, the total is about **$8.05 per month**, under the $20 budget. The same instance running 24/7 would cost about $31.04 per month, so Cost Guard avoids about $23.92 per month.

![Estimate in the AWS Pricing Calculator: $7.12 per month](../../images/5-Workshop/5.7-security/53-pricing-t3small.png)

**Check:** Monthly cost is **7.12 USD**, made of Amazon EC2 4.70 USD, Amazon EBS 2.40 USD and AWS Lambda 0.02 USD, in US East (N. Virginia).

### 3. Cost optimization

- Limit the running hours. The scheduled stop and the manual start keep the machine near 120 hours instead of 730.
- Use pay-per-use services in their free allowance: Lambda, DynamoDB on-demand, SNS and EventBridge Scheduler.
- Use standard parameters in Parameter Store, which have no charge.
- Keep the 30 GB gp3 volume and add no extra IOPS or throughput.
- Do not attach an Elastic IP, which is billed while it is allocated.
- Delete all resources after the workshop (see 5.8).

In this section, you reviewed the security of Cost Guard (one IAM role per function, `StopInstances` allowed on one instance only, no inbound ports), the estimated cost of about $7.12 a month for 120 lab hours compared with $31.04 for running 24/7, and the ways to keep the cost low.

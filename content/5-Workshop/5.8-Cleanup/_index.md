---
title: "Clean Up"
date: 2026-11-18
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

## Clean Up

Delete everything you created so that nothing is billed after the workshop. One stack holds all resources, so one delete removes them.

### Step 1 – Delete the stack

1. Open **CloudFormation → Stacks → CostGuard**.
2. Choose **Delete** and confirm.
3. Wait until the stack disappears from the list or shows **DELETE_COMPLETE**.

With the CLI:

```bash
aws cloudformation delete-stack --region us-east-1 --stack-name CostGuard
```

The stack removes the instance and its volume, the DynamoDB table, the Parameter Store parameters, the Lambda functions, the log groups, the schedules, the Budget, the SNS topics and the alarms.

If you passed `ExistingInstanceId`, the stack did **not** create that instance, so it does not delete it. Terminate it yourself.

### Step 2 – Check that nothing is left

Check each console in `us-east-1`:

- [ ] **EC2 → Instances:** no instance of the lab is running or stopped.
- [ ] **EC2 → Volumes:** no volume is left behind.
- [ ] **EC2 → Elastic IPs:** none allocated.
- [ ] **Lambda:** no `CostGuard-*` function.
- [ ] **DynamoDB:** no `CostGuardSessions` table.
- [ ] **EventBridge → Scheduler:** no `costguard-*` schedule.
- [ ] **SNS:** no `costguard-*` topic.
- [ ] **CloudWatch:** no `CostGuard-*` alarm and no `/aws/lambda/CostGuard-*` log group.
- [ ] **Budgets:** no `CostGuard-Budget`.
- [ ] **Systems Manager → Parameter Store:** no `/costguard/` parameter.

### Step 3 – Compare the cost

Billing data arrives with a delay, so wait one or two days.

1. Open **Billing and Cost Management → Cost Explorer**.
2. Choose the days of the workshop, group by **Service**, and compare the real EC2, EBS and other costs with the estimate in 5.7.
3. Check that the total is under the $20 budget.

### If a deletion fails

- **The stack stays in `DELETE_FAILED`:** open the **Events** tab, find the resource that failed, delete it by hand in its console, then delete the stack again.
- **A resource is left after the stack is gone:** delete it by hand. Resources of this workshop carry the tag `Project=CostGuard` where tags are supported.

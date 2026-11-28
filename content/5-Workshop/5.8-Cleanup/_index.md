---
title: "Clean Up"
date: 2026-11-18
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

## Clean Up

You have now completed the whole Cost Guard workshop: the lab machine is protected by three layers, and all three have been tested. Only the final step is left, which is to clean up and delete everything you created so that nothing is billed afterwards. The screenshots below come from our own clean-up on 26/11/2026 in `us-east-1`, done with an IAM user, not the root user.

Order matters: delete the `CostGuard` stack first, then the lab instance, then the few resources that were created outside the stacks.

### Step 1 – Look at what you have

1. Open **CloudFormation → Stacks**. In our account there were two stacks: `CostGuard` (the three protection layers) and `costguard-instance` (the Windows lab machine, passed to `CostGuard` as `ExistingInstanceId`).

![Two stacks before the clean-up](../../images/5-Workshop/5.8-cleanup/69-cleanup-stacks-before.png)

2. Open the **Resources** tab of each stack to see what it will delete. `CostGuard` holds 29 resources: Lambda functions, roles, log groups, schedules, the DynamoDB table, the SNS topics, the Budget and the alarms.

![Resources of the CostGuard stack](../../images/5-Workshop/5.8-cleanup/70-cleanup-costguard-resources.png)

![Resources of the costguard-instance stack: instance, launch template, security group](../../images/5-Workshop/5.8-cleanup/71-cleanup-instance-stack-resources.png)

3. Note the instance ID in **EC2 → Instances**, so you can check later that it is terminated.

![The lab instance before the clean-up](../../images/5-Workshop/5.8-cleanup/72-cleanup-ec2-before.png)

### Step 2 – Delete the CostGuard stack

1. Select `CostGuard` and choose **Delete**. Type the stack name to confirm.

![Delete confirmation for CostGuard](../../images/5-Workshop/5.8-cleanup/73-cleanup-delete-costguard-confirm.png)

![Stack name typed in the confirmation box](../../images/5-Workshop/5.8-cleanup/74-cleanup-delete-costguard-typed.png)

2. The stack goes to **DELETE_IN_PROGRESS**. The **Events** tab shows each resource being deleted.

![CostGuard in DELETE_IN_PROGRESS](../../images/5-Workshop/5.8-cleanup/75-cleanup-costguard-delete-in-progress.png)

![Events while CostGuard is being deleted](../../images/5-Workshop/5.8-cleanup/76-cleanup-costguard-events-deleting.png)

3. Wait for **DELETE_COMPLETE**. The stack disappears from the active list; to see it again, set the filter to **Deleted**.

![Active stacks after CostGuard is gone](../../images/5-Workshop/5.8-cleanup/77-cleanup-stacks-after-costguard.png)

![CostGuard in DELETE_COMPLETE](../../images/5-Workshop/5.8-cleanup/78-cleanup-costguard-delete-complete.png)

![Last events of CostGuard](../../images/5-Workshop/5.8-cleanup/79-cleanup-costguard-events-complete.png)

With the CLI:

```bash
aws cloudformation delete-stack --region us-east-1 --stack-name CostGuard
aws cloudformation wait stack-delete-complete --region us-east-1 --stack-name CostGuard
```

### Step 3 – Delete the lab instance

If `CostGuard` created the instance itself (empty `ExistingInstanceId`), Step 2 already terminated it; skip to Step 4.

If you passed `ExistingInstanceId`, the stack did **not** create that instance and does not delete it. We created it with a separate stack, so we deleted that stack:

1. Select `costguard-instance`, choose **Delete** and type the name.

![Delete confirmation for costguard-instance](../../images/5-Workshop/5.8-cleanup/80-cleanup-delete-instance-confirm.png)

![Stack name typed in the confirmation box](../../images/5-Workshop/5.8-cleanup/81-cleanup-delete-instance-typed.png)

![costguard-instance in DELETE_IN_PROGRESS](../../images/5-Workshop/5.8-cleanup/82-cleanup-instance-delete-in-progress.png)

2. The instance becomes **Terminated**. AWS hides terminated instances after about an hour; they cost nothing.

![The lab instance is terminated](../../images/5-Workshop/5.8-cleanup/83-cleanup-ec2-terminated.png)

3. The active stack list is now empty, and the **Deleted** filter shows both stacks.

![No active stack left](../../images/5-Workshop/5.8-cleanup/84-cleanup-stacks-active-empty.png)

![Both stacks under the Deleted filter](../../images/5-Workshop/5.8-cleanup/85-cleanup-stacks-deleted-list.png)

![Events of costguard-instance: instance, launch template and security group deleted](../../images/5-Workshop/5.8-cleanup/86-cleanup-instance-events-complete.png)

If you made the instance by hand, terminate it in **EC2 → Instances → Instance state → Terminate instance**.

### Step 4 – Delete what the stacks did not create

Two things were created outside the stacks during the project. Check for them even if you followed the guide exactly.

**IAM role made by the Scheduler console.** When you create a schedule in the EventBridge Scheduler console and choose *Create new role*, AWS creates a role `Amazon_EventBridge_Scheduler_LAMBDA_*` and a policy `Amazon-EventBridge-Scheduler-Execution-Policy-*`. Deleting the stack does not remove them.

1. Open **IAM → Roles**, search `Amazon_EventBridge_Scheduler`. Open the role and check that its policy only invokes a `CostGuard-*` function.

![Leftover Scheduler role](../../images/5-Workshop/5.8-cleanup/104-cleanup-leftover-scheduler-role.png)

2. Delete the role, then delete the policy in **IAM → Policies** (it has no attachment left).

![Delete the role](../../images/5-Workshop/5.8-cleanup/105-cleanup-delete-scheduler-role-confirm.png)

![Role deleted](../../images/5-Workshop/5.8-cleanup/106-cleanup-scheduler-role-deleted.png)

![Delete the policy](../../images/5-Workshop/5.8-cleanup/107-cleanup-delete-scheduler-policy-confirm.png)

![Policy deleted](../../images/5-Workshop/5.8-cleanup/108-cleanup-scheduler-policy-deleted.png)

**S3 bucket `cf-templates-*`.** When you upload a template file in the CloudFormation console, AWS stores it in a bucket named `cf-templates-<random>-us-east-1`. Delete it only if it holds nothing but this workshop's templates.

1. Open the bucket and check its objects. Ours held only `costguard` and `cost-guard` templates.

![Objects in the cf-templates bucket](../../images/5-Workshop/5.8-cleanup/109-cleanup-s3-cf-templates-objects.png)

2. Choose **Empty**, type `permanently delete`, and confirm.

![Empty the bucket](../../images/5-Workshop/5.8-cleanup/110-cleanup-s3-empty-confirm.png)

![Bucket emptied](../../images/5-Workshop/5.8-cleanup/111-cleanup-s3-empty-done.png)

3. Choose **Delete**, type the bucket name, and confirm.

![Delete the bucket](../../images/5-Workshop/5.8-cleanup/112-cleanup-s3-delete-confirm.png)

![Bucket deleted](../../images/5-Workshop/5.8-cleanup/113-cleanup-s3-bucket-deleted.png)

### Step 5 – Check that nothing is left

Check each console in `us-east-1`:

- [ ] **EC2 → Instances:** no lab instance is running or stopped (a *Terminated* line is fine).

![EC2 instances](../../images/5-Workshop/5.8-cleanup/87-check-ec2-instances.png)

- [ ] **EC2 → Volumes:** no volume is left behind.

![EBS volumes](../../images/5-Workshop/5.8-cleanup/88-check-ebs-volumes.png)

- [ ] **EC2 → Elastic IPs:** none allocated.

![Elastic IPs](../../images/5-Workshop/5.8-cleanup/89-check-elastic-ips.png)

- [ ] **EC2 → Security Groups** and **Launch Templates:** only `default` is left, no lab launch template.

![Security groups](../../images/5-Workshop/5.8-cleanup/90-check-security-groups.png)

![Launch templates](../../images/5-Workshop/5.8-cleanup/91-check-launch-templates.png)

- [ ] **Lambda:** no `CostGuard-*` function.

![Lambda functions](../../images/5-Workshop/5.8-cleanup/92-check-lambda-functions.png)

- [ ] **DynamoDB:** no `CostGuardSessions` table.

![DynamoDB tables](../../images/5-Workshop/5.8-cleanup/93-check-dynamodb-tables.png)

- [ ] **EventBridge → Scheduler** and **Rules:** no `costguard-*` schedule and no CostGuard rule.

![Scheduler schedules](../../images/5-Workshop/5.8-cleanup/94-check-scheduler-schedules.png)

![EventBridge rules on the default bus](../../images/5-Workshop/5.8-cleanup/114-check-eventbridge-rules-default-bus.png)

- [ ] **SNS:** no `costguard-*` topic and no subscription to it.

![SNS topics](../../images/5-Workshop/5.8-cleanup/96-check-sns-topics.png)

![SNS subscriptions](../../images/5-Workshop/5.8-cleanup/97-check-sns-subscriptions.png)

- [ ] **CloudWatch:** no `CostGuard-*` alarm and no `/aws/lambda/CostGuard-*` log group.

![CloudWatch alarms](../../images/5-Workshop/5.8-cleanup/98-check-cloudwatch-alarms.png)

![CloudWatch log groups](../../images/5-Workshop/5.8-cleanup/99-check-cloudwatch-log-groups.png)

- [ ] **Budgets:** no `CostGuard-Budget`. Budgets made by other people in a shared account can stay.

![Budgets](../../images/5-Workshop/5.8-cleanup/100-check-budgets.png)

- [ ] **Systems Manager → Parameter Store:** no `/costguard/` parameter.

![Parameter Store](../../images/5-Workshop/5.8-cleanup/101-check-parameter-store.png)

- [ ] **IAM → Roles:** no `CostGuard-*` role.

![IAM roles](../../images/5-Workshop/5.8-cleanup/115-check-iam-roles-costguard.png)

- [ ] **S3:** no `cf-templates-*` bucket of this workshop.

![S3 buckets](../../images/5-Workshop/5.8-cleanup/103-check-s3-buckets.png)

The same check in **CloudShell**. Each line counts the resources whose name contains `costguard`; every count must be `0`, and the last two lines (EBS volumes, Elastic IPs) must be `0` as well:

```bash
c(){ printf "%-28s %s\n" "$1" "$(eval "$2" | grep -ci costguard)"; }
c "Stacks (active)" "aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE --query 'StackSummaries[].StackName' --output text"
c "EC2 running/stopped" "aws ec2 describe-instances --filters Name=instance-state-name,Values=running,stopped --query 'Reservations[].Instances[].Tags' --output text"
c "Lambda functions" "aws lambda list-functions --query 'Functions[].FunctionName' --output text"
c "Scheduler schedules" "aws scheduler list-schedules --query 'Schedules[].Name' --output text"
c "DynamoDB tables" "aws dynamodb list-tables --output text"
c "SNS topics" "aws sns list-topics --output text"
c "CloudWatch alarms" "aws cloudwatch describe-alarms --query 'MetricAlarms[].AlarmName' --output text"
c "Log groups" "aws logs describe-log-groups --query 'logGroups[].logGroupName' --output text"
c "SSM parameters" "aws ssm describe-parameters --query 'Parameters[].Name' --output text"
c "IAM roles" "aws iam list-roles --query 'Roles[].RoleName' --output text"
c "S3 buckets" "aws s3 ls | grep -i -e costguard -e cf-templates"
c "Budgets" "aws budgets describe-budgets --account-id $(aws sts get-caller-identity --query Account --output text) --query 'Budgets[].BudgetName' --output text"
aws ec2 describe-volumes --query "length(Volumes)"
aws ec2 describe-addresses --query "length(Addresses)"
```

![CloudShell: every count is 0](../../images/5-Workshop/5.8-cleanup/116-check-cloudshell-final-sweep.png)

### Workshop demo video

Link to the workshop demo clip: <https://drive.google.com/file/d/1NrNFPlScetGuT9zNIzrrsFm-SImVmhyQ/view?usp=sharing>

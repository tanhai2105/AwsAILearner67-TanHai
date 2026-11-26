---
title: "Layer 3 – Budget Backstop and Monitoring"
date: 2026-11-18
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Layer 3 – Budget Backstop and Monitoring

Layer 3 is the last safety net. It follows the **actual spend** of the account, so it is slow by design: billing data refreshes only a few times per day. This step also covers the CloudWatch alarms that tell you when one of the three functions fails.

![Layer 3 – Budget Backstop and observability](../../images/5-Workshop/layer3-backstop.png)

### How it works

1. **AWS Budgets** tracks the monthly cost of the account against a limit of $20 (`CostGuard-Budget`).
2. When the actual cost goes above **80%** and above **100%** of the limit, Budgets sends a notification to the SNS topic `costguard-budgets` and an email to you.
3. The topic invokes the Lambda function `CostGuard-BudgetBackstop`.
4. The function stops the instance if it is running, writes `budget_stop` (or `budget_noop`) to `CostGuardSessions` and publishes a confirmation to `costguard-alerts`.

### How the layers work together

- Layer 1 stops the machine at a fixed hour.
- Layer 2 stops it within minutes when the estimated cost reaches the threshold.
- Layer 3 stops it when the real bill gets close to the limit.

Any layer can stop the machine, no layer can start it, and stopping a stopped machine does nothing. So the layers do not conflict, and one layer failing does not remove the other two.

### Step 1 – Look at the budget

1. Open **Billing and Cost Management → Budgets** and choose `CostGuard-Budget`.
2. Check the monthly limit and the two alerts (80% and 100%, actual cost).

![CostGuard-Budget: $20 monthly limit, $0.00 spent, $1.68 forecast](../../images/5-Workshop/5.6-layer3/60-budgets-costguard-detail.png)

**Check:** Budget amount is `$20.00`, Period is **Monthly** and Health status is **Healthy**.

![The two CostGuard-Budget alerts: actual cost > 80% ($16) and > 100% ($20)](../../images/5-Workshop/5.6-layer3/61-budgets-costguard-alerts.png)

**Check:** there are exactly two alerts on actual cost, at 80% ($16.00) and 100% ($20.00). **Email verification** must show the address as verified; if it is still **Pending**, choose **Resend verification** and confirm it in your mailbox, otherwise Budgets sends no email.

### Step 2 – Test the Budget Backstop

You do not wait for the real bill to grow. You send a message to the topic yourself.

1. Start the instance and wait until it is `running`.
2. Open **SNS → Topics → `costguard-budgets` → Publish message**. Use any subject and message, and choose **Publish message**.
3. Check the **EC2** console, your mailbox and the DynamoDB table.

**Result:** the instance goes to `stopping` and then `stopped`. You receive the email "CostGuard: budget notification handled". The table has an item with `event = budget_stop`.

![Publishing a test message to costguard-budgets](../../images/5-Workshop/5.6-layer3/35-cg-l3-publish-budgets.png)

**Check:** the console reports that the message was published and shows a Message ID.

![Log of CostGuard-BudgetBackstop](../../images/5-Workshop/5.6-layer3/36-cg-l3-backstop-logs.png)

**Check:** there is one invocation (`START`, `END` and `REPORT` lines) right after the publish time and no `ERROR` line.

![CostGuardSessions with a budget_stop item](../../images/5-Workshop/5.6-layer3/37-cg-l3-dynamodb-budget-stop.png)

**Check:** there is a new item with `event = budget_stop`.

![Email "CostGuard: budget notification handled" (from a repeat test while the instance was already stopped)](../../images/5-Workshop/5.6-layer3/44-gmail-alert-budget.png)

**Check:** the email subject is "CostGuard: budget notification handled".

### Step 3 – Look at the alarms

1. Open **CloudWatch → Alarms**.
2. You see three alarms: `CostGuard-ScheduleHandler-Errors`, `CostGuard-CostMeter-Errors` and `CostGuard-BudgetBackstop-Errors`. Each one watches the `Errors` metric of its function and sends a message to `costguard-errors` when the metric is above 0.
3. Open **CloudWatch → Log groups**. The three `/aws/lambda/CostGuard-*` log groups keep their logs for 14 days.

![The three CostGuard-*-Errors alarms in OK](../../images/5-Workshop/5.6-layer3/65-cloudwatch-alarms-ok.png)

**Check:** all three alarms exist, State is **OK**, Actions shows **Actions enabled** and the condition is `Errors > 0`.

![The three /aws/lambda/CostGuard-* log groups keep logs for 2 weeks](../../images/5-Workshop/5.6-layer3/66-cloudwatch-log-groups-14d.png)

**Check:** the Retention column shows **2 weeks** (14 days) for all three log groups.

### Step 4 – Test an error scenario

This test makes `CostGuard-CostMeter` fail on purpose to check that the alarm and the email work.

1. Open **Parameter Store → `/costguard/instance-id` → Edit**. Write down the real instance ID first, then replace the value with `i-00000000000000000`.
2. Wait up to 10 minutes. The function runs every 5 minutes and the alarm uses a 5-minute period.
3. Check the log of `CostGuard-CostMeter`, the alarm state and your mailbox.
4. **Put the real instance ID back.**

**Result:** the function fails, you receive an email from `costguard-errors`, and the alarm `CostGuard-CostMeter-Errors` changes to **In alarm**. After you restore the ID and the next run succeeds, the alarm returns to **OK**.

![Alarm CostGuard-CostMeter-Errors in In alarm](../../images/5-Workshop/5.6-layer3/39-cg-alarm-costmeter-in-alarm.png)

**Check:** State is **In alarm** after the failed run of the function.

![Alarm back to OK after the real ID was restored](../../images/5-Workshop/5.6-layer3/40-cg-alarm-back-ok.png)

**Check:** State is back to **OK** after the next successful run.

![ALARM email sent through costguard-errors (from the same kind of test on ScheduleHandler)](../../images/5-Workshop/5.6-layer3/46-gmail-alarm.png)

**Check:** the email subject starts with `ALARM:` and names the alarm that fired.

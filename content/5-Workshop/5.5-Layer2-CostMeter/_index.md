---
title: "Layer 2 – Cost Meter"
date: 2026-11-18
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Layer 2 – Cost Meter

Layer 2 estimates how much the lab instance has cost this month and stops it when the estimate reaches a threshold. It reacts within minutes, because it uses the running time of the instance and a configured hourly price, not billing data. This layer is the original contribution of the project.

![Layer 2 – Cost Meter](../../images/5-Workshop/layer2-costmeter.png)

### How it works

1. **EventBridge Scheduler** invokes `CostGuard-CostMeter` every 5 minutes (`rate(5 minutes)`).
2. The function reads the instance ID, the hourly price and the threshold from Parameter Store.
3. If the instance is **not running**, the function stops here.
4. If it is running, the function calculates the cost of this interval:

   ```text
   interval cost = price per hour × 5 ÷ 60
   ```

   With $0.0392 per hour, each run adds about $0.0033.
5. It reads all `sample` items of the current calendar month (UTC) from `CostGuardSessions` and adds them up. This is the monthly total.
6. It writes a new `sample` item with the interval cost and the new monthly total.
7. If the total reaches the threshold, it stops the instance, writes an `auto_stop` item and publishes an alert to `costguard-alerts`.

**What the threshold covers.** The estimate counts the running cost of the EC2 instance only. The EBS volume ($2.40 per month) is a fixed cost and is counted separately in the budget.

**After an automatic stop.** The monthly total stays above the threshold, so if you start the instance again, the next run stops it again within 5 minutes. The machine stays locked until the next calendar month. This keeps the credit safe, and the price is that the lab cannot be used for the rest of that month.

**Known limits.** A session shorter than one interval may not be counted, and the month changes at midnight UTC, which is 07:00 in Vietnam.

### Step 1 – Look at the configuration

1. Open **Systems Manager → Parameter Store** and read `/costguard/price-per-hour` (0.0392) and `/costguard/cost-threshold` (14).
2. Open **EventBridge → Scheduler → Schedules** and check that `costguard-cost-meter` is enabled and uses `rate(5 minutes)`.

![The four /costguard/ parameters in Parameter Store](../../images/5-Workshop/5.5-layer2/63-parameter-store-costguard.png)

**Check:** all four parameters exist (`budget-limit`, `cost-threshold`, `instance-id` and `price-per-hour`) in the **Standard** tier, which has no charge.

![Value of /costguard/price-per-hour](../../images/5-Workshop/5.5-layer2/64-param-price-per-hour.png)

**Check:** the value is `0.0392`. Open `/costguard/cost-threshold` the same way; its value must be `14`.

![Schedule costguard-cost-meter is enabled](../../images/5-Workshop/5.4-layer1/41-cg-schedules.png)

**Check:** `costguard-cost-meter` is **Enabled**. Open it to see the expression `rate(5 minutes)` and the target `CostGuard-CostMeter`.

### Step 2 – Test the threshold

You do not wait for the real threshold of $14. You lower it so that the next run crosses it.

1. Start the instance and wait until it is `running`.
2. Open **Parameter Store → `/costguard/cost-threshold` → Edit** and change the value to `0.001`. Write down the clock time when you save.
3. Wait for the next run, at most 5 minutes.
4. Check the **EC2** console, your mailbox and the DynamoDB table.
5. **Put the threshold back to `14`.** Do not update the CloudFormation stack during the test, because that would also reset the value.

**Result:** the instance goes to `stopping` and then `stopped`. You receive the email "CostGuard: instance stopped (cost threshold)". The table has a `sample` item and an `auto_stop` item.

![Threshold set to 0.001 in Parameter Store](../../images/5-Workshop/5.5-layer2/31-cg-l2-threshold-0001.png)

**Check:** the value is now `0.001`. Write down the time you saved it for Step 3.

![Email "CostGuard: instance stopped (cost threshold)"](../../images/5-Workshop/5.5-layer2/47-gmail-alert-costmeter.png)

**Check:** the email subject is "CostGuard: instance stopped (cost threshold)" and it arrives in the mailbox you subscribed in 5.3.

![CostGuardSessions with sample and auto_stop items](../../images/5-Workshop/5.5-layer2/34-cg-dynamodb-events.png)

**Check:** there are `event = sample` items (one per run) and one `event = auto_stop` item right after the threshold was crossed.

### Step 3 – Measure the reaction time

The goal of the project is a reaction time below 5 minutes, which is one interval of the meter.

1. Take the time when you saved the new threshold in Step 2.
2. Open **CloudTrail → Event history**, filter by event name `StopInstances` and read the event time.
3. The difference is the reaction time of this test.

**Result:** the difference is not larger than the interval of the meter. Keep the real numbers of your own run.

In our run we saved the threshold at 17:22:53, and CloudTrail recorded `StopInstances` by `CostGuard-CostMeter` at 17:26:40: a reaction time of 3 minutes 47 seconds.

![CloudTrail StopInstances event from CostGuard-CostMeter](../../images/5-Workshop/5.5-layer2/33-cg-l2-cloudtrail-reaction.png)

**Check:** the `StopInstances` event was called by `CostGuard-CostMeter`, and its event time is no more than 5 minutes after you saved the threshold.

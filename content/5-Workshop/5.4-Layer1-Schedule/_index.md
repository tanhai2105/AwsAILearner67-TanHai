---
title: "Layer 1 – Schedule Guard"
date: 2026-11-18
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Layer 1 – Schedule Guard

Layer 1 stops the lab instance at a fixed time every day. It does not read any cost or billing data, so it is the most reliable of the three layers.

![Layer 1 – Schedule Guard](../../images/5-Workshop/layer1-schedule.png)

### How it works

1. **EventBridge Scheduler** invokes the Lambda function `CostGuard-ScheduleHandler` every day at 22:00 (time zone `Asia/Ho_Chi_Minh`), with the event `{"action": "stop"}`.
2. The function reads the instance ID from `/costguard/instance-id`.
3. It calls `DescribeInstances`. If the instance is `running`, it calls `StopInstances`. In any other state, it does nothing, so a repeated call is safe.
4. It writes a record to `CostGuardSessions` (`schedule_stop` or `schedule_noop`).
5. If anything fails, it publishes the error to `costguard-errors` and raises the error, so the CloudWatch alarm in 5.6 can see it.

The schedule only **stops** the machine. Starting it stays a manual action, so no credit is spent while nobody uses the lab.

### Why EventBridge Scheduler

An EventBridge **rule** reads a cron expression in UTC, so a stop planned for 22:00 in Vietnam must be written as 15:00. EventBridge Scheduler accepts a time zone, so the expression below says exactly what it means. Scheduler also invokes the function through its own role, so the function needs no extra resource-based permission.

```yaml
StopSchedule:
  Type: AWS::Scheduler::Schedule
  Properties:
    Name: costguard-daily-stop
    ScheduleExpression: !Ref StopCron                       # default cron(0 22 * * ? *)
    ScheduleExpressionTimezone: !Ref StopScheduleTimezone   # default Asia/Ho_Chi_Minh
    FlexibleTimeWindow:
      Mode: "OFF"
    Target:
      Arn: !GetAtt ScheduleHandlerFunction.Arn
      RoleArn: !GetAtt SchedulerRole.Arn
      Input: '{"action": "stop"}'
```

### Step 1 – Look at the schedule

1. Open **EventBridge → Scheduler → Schedules** and choose `costguard-daily-stop`.
2. Check the cron expression, the time zone and the target `CostGuard-ScheduleHandler`.

![Schedules costguard-daily-stop and costguard-cost-meter are enabled](../../images/5-Workshop/5.4-layer1/41-cg-schedules.png)

**Check:** `costguard-daily-stop` and `costguard-cost-meter` are both **Enabled**. Open `costguard-daily-stop` to see the expression `cron(0 22 * * ? *)`, the time zone `Asia/Ho_Chi_Minh` and the target `CostGuard-ScheduleHandler`.

### Step 2 – Test the function

1. Make sure the instance is **running** (EC2 console).
2. Open **Lambda → CostGuard-ScheduleHandler → Test**. Create a test event with the body `{}` and choose **Test**.
3. Open the **EC2** console and refresh.
4. Open **DynamoDB → Tables → CostGuardSessions → Explore table items**.

**Result:** the function returns `schedule_stop` with `state_before` set to `running`. The instance goes from `stopping` to `stopped`. The table has a new item with `event = schedule_stop`.

![Test result: schedule_stop with state_before running](../../images/5-Workshop/5.4-layer1/29-cg-l1-schedule-stop.png)

**Check:** the test status is **Succeeded** and the result contains `schedule_stop` with `state_before` set to `running`.

Run the test a second time while the instance is stopped. **Result:** the function returns `schedule_noop` and the instance is not touched.

![Second test: schedule_noop with state_before stopped](../../images/5-Workshop/5.4-layer1/30-cg-l1-schedule-noop.png)

**Check:** this time the result is `schedule_noop` with `state_before` set to `stopped`, so the function did not call `StopInstances` again.

### Step 3 – Test the real schedule (optional)

1. Start the instance again.
2. Open **CloudFormation → CostGuard → Update**, keep the current template and change `StopCron` to a time a few minutes ahead, for example `cron(25 15 * * ? *)` at 15:25 Vietnam time.
3. Wait until that time and check the instance state.
4. Update the stack again and put `cron(0 22 * * ? *)` back.

**Result:** the instance is stopped at the chosen time in Vietnam time, not seven hours earlier or later.

In our own run we did not change `StopCron`; we checked the real 22:00 run on 22/11/2026 instead. The schedule invoked the function at 15:00:02 UTC (22:00 Vietnam time). The instance was already stopped, so the function only wrote `schedule_noop`.

![Log of the 22:00 run: schedule_noop](../../images/5-Workshop/5.4-layer1/58-schedulehandler-22h-noop.png)

**Check:** the log shows an invocation at 15:00 UTC (22:00 Vietnam time) and a `schedule_noop` line with the instance ID.

![DynamoDB item of the 22:00 run](../../images/5-Workshop/5.4-layer1/59-dynamodb-22h-noop-record.png)

**Check:** there is a new item with `event = schedule_noop` and a `timestamp` that matches the schedule run.

In this section, you looked at the schedule `costguard-daily-stop`, which runs at 22:00 Vietnam time through EventBridge Scheduler, and tested the `CostGuard-ScheduleHandler` function. When the instance was running, the function stopped it and wrote `schedule_stop`; when it was already stopped, the function only wrote `schedule_noop`. Layer 1 stops the machine at a fixed time and needs no billing data.

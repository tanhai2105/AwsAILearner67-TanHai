---
title: "Blog 4"
date: 2026-11-26
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

### STOPPING EC2 AT 22:00 VIETNAM TIME WITH EVENTBRIDGE SCHEDULER + LAMBDA (AND THE EASY MISTAKES)

**Published on:** AWS Study Group (Facebook)

This is my part of my team's Cost Guard project: Layer 1, which stops the Windows lab instance every night. It sounds simple, but a few mistakes make the instance stop at the wrong time or not at all. Here is how we built it and what I learned.

![Layer 1 – Schedule Guard: EventBridge Scheduler calls the ScheduleHandler Lambda at 22:00, the function reads the instance ID from Parameter Store, stops the EC2 instance and logs the event in DynamoDB](../../images/3-BlogsPosted/3.4-Blog4/layer1-schedule.png)

**Flow:** EventBridge Scheduler (22:00, `Asia/Ho_Chi_Minh`) → Lambda `CostGuard-ScheduleHandler` → read the instance ID from Parameter Store → if the instance is running, call `StopInstances`, otherwise skip → log the event in DynamoDB → on error, send an email through the SNS topic `costguard-errors`.

**Key points:**

- **Time zone: use Scheduler, not an old-style cron rule.** Cron in classic EventBridge rules runs in UTC. To stop at 22:00 Vietnam time (UTC+7) you must write 15:00 UTC, which is easy to forget and off by 7 hours. EventBridge Scheduler lets you pick the time zone on the schedule itself. We use the expression `cron(0 22 * * ? *)`, the time zone `Asia/Ho_Chi_Minh`, and Flexible time window turned off so it runs at exactly 22:00.
- **Make the Lambda safe to run any number of times.** The function checks the instance state before stopping it: if running, it calls `StopInstances` and records `schedule_stop`; if already stopped, it does nothing and only records `schedule_noop`. Every run writes a row to DynamoDB with the previous state, so we can test as often as we like, and the table shows which nights the instance was really stopped.
- **Least privilege.** The function's role may call `StopInstances` only on the ARN of the one lab instance. `DescribeInstances` cannot be limited by resource, so it uses `*`, but it is read-only. Three people share the lab account, and limiting the ARN keeps the function from stopping a teammate's instance. Scheduler also needs its own role (`scheduler.amazonaws.com`) with only `lambda:InvokeFunction` on the target function; without it the schedule still shows Enabled but the function is never called.
- **Know when it fails.** On an error, the function emails through SNS `costguard-errors` and then re-raises. Re-raising makes the Lambda Errors metric go up so the CloudWatch alarm can catch it; swallowing the error means the alarm never fires. We tested this by temporarily setting `/costguard/instance-id` to an ID that does not exist: about 6 minutes later the alarm `CostGuard-CostMeter-Errors` went to In alarm, and it returned to OK once the real ID was restored.
- **Test without waiting until 22:00.** Invoke the function with the same payload the schedule sends: `{"action": "stop"}`. In our test on 25/11, the instance went from running to stopping in about 3 seconds and the function returned `{"event": "schedule_stop", "state_before": "running"}`. A second call while the instance is stopped must return `schedule_noop`. After 22:00, check the function's Monitoring tab or the DynamoDB table to make sure the real schedule ran.

**Conclusion and thoughts:**

For local time, use EventBridge Scheduler with a time zone. Check the state before acting and record every run. Limit the stop permission to the instance ARN, and re-raise errors so the alarm sees them. This layer is small, but it is the most reliable part of Cost Guard because it does not depend on billing data.

My team's full workshop: [nguyenducbinh-aws.github.io/AwsAILearner67](https://nguyenducbinh-aws.github.io/AwsAILearner67/)

**References:**

- [EventBridge Scheduler: schedule types and time zones](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [EC2 StopInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_StopInstances.html)

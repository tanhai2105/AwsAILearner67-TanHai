---
title: "Blog 4"
date: 2026-11-26
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

### AMAZON EVENTBRIDGE SCHEDULER: RUN TASKS ON A SCHEDULE ON AWS WITHOUT A SERVER

**Published on:** AWS Study Group (Facebook)

I am an intern at AWS and spent time reading the documentation of Amazon EventBridge Scheduler. It is the service for running something at a set time (call a Lambda function, publish to SNS, stop an EC2 instance...) without building your own server or cron. This post is a summary so that anyone new to AWS can get the big picture quickly.

**Key points:**

- **What it is:** a serverless scheduler that lets you create, run and manage tasks from one central service. According to the AWS documentation, a schedule can invoke more than 270 AWS services and more than 6,000 API operations, and the service can handle millions of schedules without you managing infrastructure. AWS recommends Scheduler over EventBridge scheduled rules, because it scales better and supports a wider set of target APIs.
- **Three schedule types:**
  - **Rate:** runs at a regular interval, for example `rate(5 minutes)`. The unit is minutes, hours or days.
  - **Cron:** runs at a specific time, using six fields (minutes, hours, day-of-month, month, day-of-week, year). For example `cron(0 22 * * ? *)` is 22:00 every day. You cannot use `*` in both day-of-month and day-of-week; one of them must be `?`.
  - **One-time:** runs once, written as `at(yyyy-mm-ddThh:mm:ss)`. A one-time schedule still counts against the account quota after it has run, so AWS recommends deleting it when it is no longer needed.
  - All types have 60-second precision: if you set 1:00, the target is invoked between 1:00:00 and 1:00:59 (when no flexible time window is set).
- **Time zones and daylight saving time:** for cron and one-time schedules, you choose a time zone from the IANA database, for example `Asia/Ho_Chi_Minh`. This is a big difference from the old-style rules, which use UTC. Scheduler also handles daylight saving time: when the clock moves forward, a run that falls on a time that does not exist is skipped; when the clock moves back, the schedule runs only once. Time zones without daylight saving time (such as Vietnam) and UTC are not adjusted.
- **Flexible time window and retries:** a flexible time window spreads the runs over a period of time instead of one exact moment, which suits work that does not need to be precise; turn it Off if it must run on time. For retries, Scheduler delivers tasks "at least once". You can set the number of retries and the maximum retention time for a failed invocation, and retries are delayed so the target has time to become available. Because a call can happen more than once, the target function should be safe to run again.
- **Targets and permissions:** *templated targets* are ready-made patterns for SQS, SNS, Lambda and EventBridge; *universal targets* call any API of the 270+ services. Scheduler needs an **execution role** to call the target API for you: when you create a schedule in the console, AWS sets up the role from the target you chose, but with the CLI, an SDK or CloudFormation you must already have a role with the required permissions.
- **Quotas:** by default each Region allows up to 10,000,000 schedules (adjustable), plus limits on create-schedule requests and invocations per second. Check Service Quotas before using it at large scale.

**Conclusion and thoughts:**

EventBridge Scheduler is a good replacement for running your own cron: it has time zones, retries, several schedule types and can call a great many services. What I find most useful is choosing the time zone on the schedule itself, and having to grant an execution role with the right permissions. I would use rate for steady repetition, cron when a specific time is needed, and always write the target so it can safely run again, because Scheduler delivers "at least once".

**References:**

- [What is Amazon EventBridge Scheduler?](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html)
- [Schedule types, time zones and daylight saving time](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [Quotas for EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/scheduler-quotas.html)

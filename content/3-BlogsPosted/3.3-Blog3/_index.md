---
title: "Blog 3"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

### AMAZON CLOUDWATCH: THE "EYES" THAT MONITOR YOUR WHOLE SYSTEM ON AWS

**Published on:** AWS Study Group (Facebook)

Deploying an application to the cloud is only the first step. After that you need to know whether the system is healthy, ideally before users complain. I am an intern at AWS and spent time reading the documentation of Amazon CloudWatch. This post is a summary so that anyone new to AWS can get the big picture quickly.

![Amazon CloudWatch: data sources (EC2, ECS, EKS, Lambda, RDS, OpenTelemetry applications) flow into CloudWatch, and the outputs are dashboards, alarms, SNS, Auto Scaling, investigations and SLOs](../../images/3-BlogsPosted/3.3-Blog3/cloudwatch-overview.png)

**Key points:**

- **What CloudWatch is:** the monitoring and observability service of AWS. CloudWatch collects metrics, logs and traces from AWS resources and from your own applications, lets you watch them in near real time, set alarms and react automatically to abnormal behavior. Many AWS services send basic metrics to CloudWatch with no extra setup.
- **Metrics and dashboards:** metrics are performance data over time (CPU, latency, request count). Use the built-in metrics or push custom metrics through OpenTelemetry (OTLP) or the PutMetricData API. Dashboards put metric and log charts on one screen and support several Regions and accounts.
- **Alarms:** watch a metric against a threshold, then send a notification (for example through SNS) or trigger an action such as Auto Scaling. A composite alarm combines several alarms into one to reduce alert noise.
- **Logs:** stored in log groups and log streams, queried with Logs Insights, with anomaly detection in logs, metric filters (turn logs into metrics) and subscription filters (send logs to S3 or Firehose).
- **Application and infrastructure monitoring:** Application Signals collects request count, latency and errors without code changes, with an application map and SLOs. Synthetics (canaries) and RUM find problems before users do and measure real experience. There are also Database Insights, Lambda Insights and Container Insights. The CloudWatch agent collects system metrics, logs and traces from EC2 and from on-premises servers.
- **What is new:** support for OpenTelemetry and PromQL (one set of instrumentation, no lock-in to a private format); cross-account observability (a central account views the data of many accounts, linked through AWS Organizations); CloudWatch Omni, an AI-assisted observability experience that lets you ask in natural language to build queries and track the quality of AI agent responses; and monitoring of workloads outside AWS.
- **Notes for real use:** start with the most important metrics and alarms (CPU, errors, latency) and grow from there; use composite alarms to avoid alarm fatigue; cost depends on the amount of metrics, logs and features you use, so read the pricing page and the AWS guidance on cost optimization.

**Conclusion and thoughts:**

CloudWatch is useful because it connects three kinds of data: metrics show how the system behaves over time, logs show why, and alarms tell you when to look. Cost grows with the amount of metrics and logs, so I would start with a few indicators such as CPU, errors and latency, and add more only when they are needed.

**References:**

1. [What is Amazon CloudWatch (User Guide)](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
2. [Amazon CloudWatch product page](https://aws.amazon.com/cloudwatch/)
3. [CloudWatch features](https://aws.amazon.com/cloudwatch/features/)
4. [Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
5. [Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html)
6. [Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)
7. [Application Signals](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Application-Monitoring-Sections.html)
8. [Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)
9. [Cross-account observability](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Cross-Account-Methods.html)
10. [OpenTelemetry](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-OpenTelemetry-Sections.html)
11. [CloudWatch Omni](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-omni.html)
12. [Pricing](https://aws.amazon.com/cloudwatch/pricing/)

**Link:** [Post on AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2341916996573259)

---
title: "Week 7 Worklog"
date: 2026-10-19
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives:

* Get comfortable with monitoring on AWS through Amazon CloudWatch (metrics, logs, alarms, dashboards).
* Understand hybrid DNS with Route 53 Resolver by building the lab end to end.
* Pick a workshop topic by comparing several ideas against the FCAJ scoring rules.
* Review Amazon S3 basics and keep my own notes.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Go through the CloudWatch introduction <br>- **Practice:** <br>&emsp; + Launch the lab stack (three EC2 instances running the CloudWatch agent) with CloudFormation <br>&emsp; + Look at the metrics and try search expressions, math expressions and dynamic labels | 10/19/2026 | 10/19/2026 | <https://000008.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Query the application logs in CloudWatch Logs Insights <br>&emsp; + Add a metric filter for error lines <br>&emsp; + Set an alarm that emails me via Amazon SNS | 10/20/2026 | 10/20/2026 | <https://000008.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + Build a CloudWatch dashboard, then delete the lab resources <br>- Compare four possible workshop topics against the FCAJ scoring rules: media moderation with Rekognition, security alerts to Telegram, a serverless REST API, automated backup and DR | 10/21/2026 | 10/21/2026 | <https://000008.awsfcaj.com/> |
| 5 | - Read about hybrid DNS and Route 53 Resolver <br>- **Practice:** <br>&emsp; + Create a key pair and launch the network stack <br>&emsp; + Open the security groups and log in through the Remote Desktop Gateway <br>&emsp; + Set up AWS Managed Microsoft AD as the "on-premises" DNS | 10/22/2026 | 10/22/2026 | <https://000010.awsfcaj.com/> |
| 6 | - **Practice:** <br>&emsp; + Add the outbound endpoint, the resolver rule and the inbound endpoint <br>&emsp; + Check resolution with nslookup and Resolve-DnsName, then clean up <br>- Study the lab "Getting started with Amazon S3" and write notes on buckets vs objects | 10/23/2026 | 10/23/2026 | <https://000010.awsfcaj.com/> <br> <https://000057.awsstudygroup.com/> |


### Week 7 Achievements:

* CloudWatch now makes sense to me as one place for metrics, logs and alarms:

  * The lab stack gave the agent an IAM role with only the permissions it needs, not AdministratorAccess
  * I combined metrics with search and math expressions and used dynamic labels to make the graphs readable
  * Logs Insights let me filter the application logs quickly; my metric filter turned ERROR lines into a metric, and I noticed it only counts events that come in after the filter exists
  * The alarm on that metric sent me an email through SNS, and the dashboard put everything on one screen

* Built the hybrid DNS lab:

  * Network (VPC, public/private subnets, Remote Desktop Gateway) from a CloudFormation template
  * AWS Managed Microsoft AD with the domain onprem.example.com playing the on-premises DNS
  * Outbound endpoint for queries going from AWS to on-premises, inbound endpoint for the reverse direction, and a resolver rule choosing which domains to forward
  * Confirmed from the gateway that names resolve with both nslookup and Resolve-DnsName

* Compared four workshop ideas (Rekognition media moderation, Telegram security alerts, serverless REST API, automated backup and DR) against the FCAJ scoring rules, which gave me a clearer view of what a good workshop topic needs.

* Wrote my own S3 notes: a bucket is the container with a globally unique name in one Region, an object is the file plus its metadata, addressed by its key.

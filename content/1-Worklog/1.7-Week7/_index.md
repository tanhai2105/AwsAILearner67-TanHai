---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Week 7 Objectives:

* Work with AWS from the terminal using the AWS CLI instead of the console.
* Create S3, SNS, IAM, network and EC2 resources entirely by command.
* Keep the workshop moving and re-check the proposal's architecture and details.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Keep working on the workshop <br>- Re-read the proposal and check the architecture against it | 10/26/2026 | 10/26/2026 | |
| 3 | - Read how the AWS CLI works: profiles, config files, output formats <br>- **Practice:** <br>&emsp; + Install the CLI and run aws configure for my profile <br>&emsp; + List my existing resources from the terminal | 10/27/2026 | 10/27/2026 | <https://000011.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + S3 commands: create a bucket, upload and list objects <br>&emsp; + SNS commands: topic and subscription | 10/28/2026 | 10/28/2026 | <https://000011.awsfcaj.com/> |
| 5 | - **Practice:** <br>&emsp; + IAM by command: group, user, access key <br>&emsp; + Networking by command: VPC, subnets, internet gateway, route table | 10/29/2026 | 10/29/2026 | <https://000011.awsfcaj.com/> |
| 6 | - **Practice:** <br>&emsp; + Key pair, security group and an EC2 instance launched from the CLI, then SSH in <br>&emsp; + Terminate everything and remove leftover resources | 10/30/2026 | 10/30/2026 | <https://000011.awsfcaj.com/> |


### Week 7 Achievements:

* Moved the workshop forward and went back over the proposal to make sure the architecture and the details still match.

* Understood that the AWS CLI is an open-source client calling the same public APIs as the console, so anything I click can also be scripted.

* Set up the CLI properly:

  * aws configure stores the access key, secret key, default Region and output format; extra named profiles are picked with --profile
  * json and yaml suit scripts, table and text are easier for me to read by eye
  * The auto-prompt feature helped a lot while I was still learning the syntax

* Created resources only from the terminal:

  * An S3 bucket with objects and an SNS topic with a subscription
  * An IAM group, a user and an access key
  * A full small network: VPC, subnets, internet gateway, route table
  * An EC2 instance in that network with its own key pair and security group, reached over SSH and then terminated

* Took the credential lessons seriously: access keys are never shared, IAM roles are preferred, and unused keys and key pairs get deleted.

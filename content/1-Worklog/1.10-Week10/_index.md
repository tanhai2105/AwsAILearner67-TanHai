---
title: "Week 10 Worklog"
date: 2026-11-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---


### Week 10 Objectives:

* Protect a web application with AWS WAF.
* Use Amazon GuardDuty to detect threats and respond to them, including automatic responses with Amazon EventBridge and AWS Lambda.
* Plan my part of the team's Windows Server on AWS project.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Read the team's messages and documents, summarize my own tasks <br>- Build the team plan for the Windows Server on AWS project, splitting the work into 3 parallel parts <br>- Check my attendance on the FCAJ Portal <br>- **Practice:** <br>&emsp; + GuardDuty scenario 1: compromised EC2 instance, auto-response with EventBridge and Lambda | 11/09/2026 | 11/09/2026 | <https://000098.awsfcaj.com/> |
| 3 | - Read how Amazon GuardDuty works: data sources and findings <br>- **Practice:** <br>&emsp; + Prepare the lab environment from the CloudFormation template | 11/10/2026 | 11/10/2026 | <https://000098.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + GuardDuty scenario 2: compromised IAM credentials, handled by hand | 11/11/2026 | 11/11/2026 | <https://000098.awsfcaj.com/> |
| 5 | - **Practice:** <br>&emsp; + GuardDuty scenario 3: IAM role credential exfiltration, handled with Lambda <br>&emsp; + Review the findings and delete the lab resources | 11/12/2026 | 11/12/2026 | <https://000098.awsfcaj.com/> |
| 6 | - Read the AWS WAF introduction <br>- **Practice:** <br>&emsp; + Host a sample web app from an S3 bucket <br>&emsp; + Create a web ACL with managed rules, then a custom rule and an advanced custom rule <br>&emsp; + Send test requests, enable logging, clean up | 11/13/2026 | 11/13/2026 | <https://000026.awsfcaj.com/> |


### Week 10 Achievements:

* For the team project, turned scattered messages and documents into a list of my own tasks and a plan with 3 parts that can run in parallel; my attendance on the FCAJ Portal is checked.

* Understood GuardDuty as a regional, fully managed detector:

  * It reads VPC Flow Logs, CloudTrail and DNS logs without touching the watched resources
  * Each finding has a readable type name and triggers a notification when it changes
  * Cost depends on CloudTrail event count and flow/DNS log volume, with a 30-day trial per Region

* Worked through the three threat scenarios:

  * Compromised EC2 (UnauthorizedAccess:EC2/MaliciousIPCaller.Custom) answered automatically by EventBridge and Lambda
  * Compromised IAM credentials (Recon:IAMUser/MaliciousIPCaller.Custom, UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom), tracking down the caller and fixing it by hand
  * Exfiltrated role credentials (UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration) handled with Lambda

* Learned that AWS WAF filters HTTP requests before they reach an application or API, blocking common attacks like SQL injection and cross-site scripting, and that I can write my own allow/block rules.

* In the WAF lab I created a web ACL with managed rules, added a rule that blocks requests carrying a certain header plus an advanced custom rule, watched the blocked test requests show up in the dashboard and sampled requests, and turned on logging.

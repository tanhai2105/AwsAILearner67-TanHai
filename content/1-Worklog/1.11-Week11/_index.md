---
title: "Week 11 Worklog"
date: 2026-11-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---


### Week 11 Objectives:

* Finish Experience 2 (Windows security on AWS) in the shared lab account and write it up as blog drafts.
* Start the new team topic "Cost Guard for Windows Labs" and deliver my part: foundation, Layer 1 (scheduled stop), pricing, workshop 5.1–5.4 and proposal fixes.
* Move my work onto the team's shared CloudFormation template.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Experience 2 in the shared lab account (us-east-1, resources prefixed `exp2-`): <br>&emsp; + Set a budget, create an IAM role for the instance, an SNS topic with an email subscription <br>&emsp; + EC2 Image Builder recipe with the STIG component and a pipeline (first build failed: build instance type not Free Tier eligible; second build produced an AMI) <br>&emsp; + Turn on AWS Config with rules `required-tags` and `approved-amis-by-id` <br>&emsp; + EventBridge rule sending Config NON_COMPLIANT to SNS, tested with 2 EC2 instances | 11/16/2026 | 11/17/2026 | |
| 3 | - Continue Experience 2: <br>&emsp; + Session Manager into the instance to check IMDSv2 <br>&emsp; + Amazon Inspector was blocked on the Free plan, so scan with SSM Patch Manager instead <br>&emsp; + RDP through Fleet Manager <br>&emsp; + Write a CloudFormation template and a least-privilege IAM policy <br>&emsp; + Clean up all resources <br>- Write 3 blog drafts | 11/17/2026 | 11/17/2026 | |
| 4 | - Team switches to "Cost Guard for Windows Labs" (3 serverless layers that stop a Windows EC2 instance to protect credit) <br>- Take my part: foundation, Layer 1 (scheduled stop), pricing, workshop 5.1–5.4, proposal fixes | 11/18/2026 | 11/18/2026 | |
| 5 | - Deploy the foundation stack (IAM roles, SNS, DynamoDB, Parameter Store) and the Windows EC2 lab instance <br>- Deploy Layer 1: EventBridge Scheduler at 22:00 Asia/Ho_Chi_Minh calling the Lambda ScheduleHandler <br>- Test the scheduled stop | 11/19/2026 | 11/19/2026 | |
| 6 | - Move to the team's shared template `cost-guard.yaml` <br>- Change the instance to t3.small and EBS gp2 to gp3, redo the pricing in the AWS Pricing Calculator <br>- Check the 22:00 schedule of the new stack <br>- Push fixes and screenshots to the team repo, fix the architecture diagram | 11/20/2026 | 11/22/2026 | |


### Week 11 Achievements:

* Completed Experience 2 end to end and cleaned up every `exp2-` resource afterwards:

  * Image Builder taught me a practical lesson: the first build failed only because the build instance type was not Free Tier eligible; after changing it, the second build produced a STIG-hardened AMI
  * AWS Config (`required-tags`, `approved-amis-by-id`) plus an EventBridge rule now pushes NON_COMPLIANT results to SNS, and the test with 2 EC2 instances confirmed it
  * Checked IMDSv2 through Session Manager and connected with Fleet Manager RDP, without opening RDP to the internet
  * Inspector was not available on the Free plan, so I used an SSM Patch Manager scan instead
  * Wrapped the setup in a CloudFormation template with a least-privilege IAM policy

* Wrote 3 blog drafts: Image Builder on the Free Tier; AWS Config + EventBridge + SNS alerts; managing Windows with SSM without opening RDP, with least privilege.

* Deployed my Cost Guard part: the foundation stack (IAM roles, SNS, DynamoDB, Parameter Store), the Windows EC2 lab instance and Layer 1 (EventBridge Scheduler at 22:00 Asia/Ho_Chi_Minh → Lambda ScheduleHandler); the scheduled stop worked in my test.

* On 11/22 switched to the shared `cost-guard.yaml`: the instance is now t3.small with a gp3 volume instead of gp2, the new pricing is $7.12/month for 120 lab hours versus $31.04 for running 24/7, and the 22:00 schedule of the new stack is confirmed. Fixes, screenshots and the corrected architecture diagram are in the team repo.

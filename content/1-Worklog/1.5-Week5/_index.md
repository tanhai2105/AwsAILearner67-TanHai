---
title: "Week 5 Worklog"
date: 2026-10-05
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---


### Week 5 Objectives:

* Brainstorm with my team what our proposal and workshop could be about.
* Learn Amazon RDS and run a MySQL database on it end to end.
* Read two AWS blogs: Fleet Manager for Windows access and CloudWatch Omni for watching AI agents.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Read the blog on AWS Systems Manager Fleet Manager | 10/05/2026 | 10/05/2026 | <https://aws.amazon.com/vi/blogs/mt/console-based-access-to-windows-instances-using-aws-systems-manager-fleet-manager/> |
| 3 | - Read the blog on Amazon CloudWatch Omni | 10/06/2026 | 10/06/2026 | <https://aws.amazon.com/vi/blogs/aws/introducing-amazon-cloudwatch-omni-ai-powered-observability-for-generative-ai-and-agentic-workloads/> |
| 4 | - Read the RDS material: <br>&emsp; + Which database engines RDS supports <br>&emsp; + Multi-AZ and Read Replicas <br>&emsp; + Automated backups and snapshots <br>&emsp; + Security with encryption, VPC and IAM | 10/07/2026 | 10/07/2026 | <https://000005.awsfcaj.com/> |
| 5 | - **Practice:** <br>&emsp; + VPC, Security Groups and a DB subnet group <br>&emsp; + An RDS MySQL instance <br>&emsp; + An EC2 instance connecting to RDS with the MySQL client <br>&emsp; + Created a database and a table, inserted rows <br>&emsp; + Deployed an app that uses RDS <br>&emsp; + Snapshot and restore <br>&emsp; + Cleanup | 10/08/2026 | 10/08/2026 | <https://000005.awsfcaj.com/> |
| 6 | - Team brainstorming session for the proposal and the workshop topic | 10/09/2026 | 10/09/2026 |  |


### Week 5 Achievements:

* The team now has a short list of first ideas for the proposal and the workshop to keep working on.

* RDS takes over the routine database work (patching, backups, failover). I learned the engines it supports, how Multi-AZ keeps a standby copy in another AZ, and how encryption, the VPC and IAM protect it.

* Ran the full RDS lab:

  * The MySQL instance sits in private subnets picked by the DB subnet group, so only the EC2 instance in the VPC can reach it
  * From EC2 I used the MySQL client to make a database and a table and add some rows
  * Deployed an application on top of that database
  * Took a snapshot and restored it as a new instance to see that the data came back

* Takeaways from the Fleet Manager blog:

  * I can RDP into a Windows instance straight from the browser, through Session Manager
  * No open RDP port, public IP or bastion host is needed, which shrinks the attack surface
  * Login works with an EC2 key pair or Windows credentials, and up to 4 instances can be shown side by side
  * The instance needs the SSM Agent, an instance profile with Systems Manager permissions and a path to the SSM endpoint

* Takeaways from the CloudWatch Omni blog:

  * AI agents can take a different path on each run, so tracing every LLM call and tool call is how you find the step that failed
  * Latency and error rate do not tell if an answer is good; Omni has 17 built-in evaluators such as coherence, helpfulness and faithfulness for that
  * Playground, Experiments and Prompt Management help compare prompts or settings and go back to an older version
  * It runs in VS Code, Kiro or the web, builds on OpenInference and ADOT, and supports LangChain, CrewAI, Strands and other frameworks

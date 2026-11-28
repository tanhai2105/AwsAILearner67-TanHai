---
title: "Week 1 Worklog"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Week 1 Objectives:

* Settle into the First Cloud AI Journey (FCAJ) program: meet the cohort, learn the office rules and set up my FCAJ Portal account.
* Build a first picture of AWS and the Management Console, start the FCAJ Workforce Bootcamp 2026 video series and get Kiro running on my laptop.
* Find my way around the Cloud Journey site and do the first Explore labs: Free Tier, AWS Budget, AWS Support and IAM.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - First day at the program: introductions with the FCAJ members <br> - Went through the AWS office rules and wrote down the points I wanted to ask the mentors about <br> - Explored the Cloud Journey website to see how its labs and guides are organized | 09/14/2026 | 09/14/2026 | <https://hcm-rules.awsfcaj.com/instructions> <br> <https://cloudjourney.awsfcaj.com/> |
| 3 | - Signed up on the FCAJ Portal and checked how to book office days and register for the uniform <br> - Opened the FCAJ Workforce Bootcamp 2026 playlist on the AWS Study Group YouTube channel and watched the opening video, Prologue - Know before you join First Cloud AI Journey | 09/15/2026 | 09/15/2026 | <https://hcm-rules.awsfcaj.com/instructions/portal> <br> <https://youtube.com/playlist?list=PLahN4TLWtox3TSYFbN1DNX7NZgTVXNj3x> <br> <https://youtu.be/Gz56QzLQ_Yo> |
| 4 | - Module 01-01 - Introduction to AWS: what cloud computing is and how AWS infrastructure is spread around the world <br> - Module 01-02 - Management Console: root user versus IAM user, and the different ways to talk to AWS <br> - Module 01-03 - Gen AI on AWS - Kiro: an intro to agentic AI and the Kiro tool <br> - **Practice:** set up Kiro IDE and Kiro CLI on my machine | 09/16/2026 | 09/16/2026 | <https://youtu.be/qVCF7UjYC5s> <br> <https://youtu.be/95quNuhvMT0> <br> <https://youtu.be/uAQCm4sm_1c> |
| 5 | - Module 01-04 - Cost optimization on AWS: the main levers for cutting cost <br> - Began Module 1 - Explore on Cloud Journey <br> - Lab AWS Free Tier 2026: how the Free Tier plans work and setting up an AWS account | 09/17/2026 | 09/17/2026 | <https://youtu.be/UIw8UxGZCHA> <br> <https://cloudjourney.awsfcaj.com/1-explore/> <br> <https://000001.awsfcaj.com/> |
| 6 | - Lab Cost Management with AWS Budget: budget types and cost alerts <br> - Lab AWS Support Request: the support plans and how a case is opened <br> - Lab Access Management with AWS IAM: groups, users, roles and role switching | 09/18/2026 | 09/18/2026 | <https://000007.awsfcaj.com/> <br> <https://000009.awsfcaj.com/> <br> <https://000002.awsfcaj.com/> |


### Week 1 Achievements:

* Now know the office rules and what is expected of interns when working on site, and cleared up a few questions with the mentors. I also know how Cloud Journey is laid out, so I can find the lab guide and reading material for each topic quickly.

* Have a working FCAJ Portal account; I know where to book office days and where to register for the uniform.

* Understood the main AWS ideas from Module 01:

  * Cloud means renting resources on demand and paying only for what I use
  * AWS is organized into Regions, each with several Availability Zones, plus Edge Locations closer to users
  * The root user should be locked away; daily work is done with IAM users
  * AWS can be used through the Console, the CLI or the SDKs

* Got the idea behind Kiro: it works from a spec before writing code (spec-driven development), and it comes as both an IDE and a CLI. Kiro IDE and Kiro CLI are installed and open without errors on my laptop.

* Cost optimization makes more sense to me: pick the right size, turn off what is not used, and watch the bill early instead of at the end of the month. I set up my own AWS account on the Free Tier and understood the difference between the Free plan, the Paid plan and the credits that come with them.

* Put a budget on the account with AWS Budget and set alert thresholds, and walked through AWS Support: what each support plan covers and the steps to open a request.

* Practiced IAM:

  * Made a group, added users to it and attached policies to the group rather than to each user
  * Created a role and switched into it to get temporary permissions
  * Tried to give each identity only what it needs (least privilege)

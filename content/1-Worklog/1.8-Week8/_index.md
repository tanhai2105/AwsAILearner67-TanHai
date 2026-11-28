---
title: "Week 8 Worklog"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


### Week 8 Objectives:

* Learn how Windows Server workloads run on AWS, since our team topic is Windows Server on AWS.
* Understand how AWS Systems Manager Automation upgrades Windows Server in place.
* Deploy WordPress on AWS with a public web tier, a private database, scaling and a CDN.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Study the AWS workshop for Microsoft workloads on AWS: its tracks, modules, prerequisites and cost <br>- **Practice (WordPress lab):** <br>&emsp; + VPC, subnets and separate security groups for the web and database tiers <br>&emsp; + EC2 instance and RDS database | 11/02/2026 | 11/02/2026 | <https://catalog.us-east-1.prod.workshops.aws/workshops/6932a032-aeec-4dfd-bad8-7bfbc8dad4c2/en-US> <br> <https://000021.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Install WordPress on EC2 and point it at the RDS database | 11/03/2026 | 11/03/2026 | <https://000021.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + Make an AMI and a launch template from the working server <br>&emsp; + Add a target group, a load balancer and an Auto Scaling group | 11/04/2026 | 11/04/2026 | <https://000021.awsfcaj.com/> |
| 5 | - **Practice:** <br>&emsp; + Take a database snapshot and restore it <br>&emsp; + Put CloudFront in front of the site <br>&emsp; + Delete all lab resources | 11/05/2026 | 11/05/2026 | <https://000021.awsfcaj.com/> |
| 6 | - Read the blog on automating Windows Server upgrades with AWS Systems Manager and note the prerequisites and limits | 11/06/2026 | 11/06/2026 | <https://aws.amazon.com/vi/blogs/modernizing-with-aws/how-to-automate-your-microsoft-windows-server-upgrades-using-aws-systems-manager-update/> |


### Week 8 Achievements:

* Went through the Microsoft workloads on AWS workshop and noted which tracks and modules fit our Windows Server topic, what each one needs before starting, and what it costs to run.

* Built WordPress on AWS step by step:

  * Web server in a public subnet, RDS in a private subnet, each with its own security group
  * WordPress installed and connected to the database
  * AMI, launch template, target group, load balancer and Auto Scaling group so the site can grow and share traffic
  * Snapshot and restore of the database, then CloudFront for faster delivery

* Saw how CloudFront, Auto Scaling with a load balancer, and RDS Multi-AZ each cover a different part of availability and performance.

* Notes from the Windows Server upgrade blog:

  * Windows Server 2012/2012 R2 lost support on October 10, 2023, and extended support for 2016 ends on January 12, 2027, after which no more security patches arrive
  * The AWSEC2-CloneInstanceAndUpgradeWindows runbook works on a clone, not the original, and outputs a new AMI
  * Before running it: SSM Agent working, TLS 1.2, 20 GB or more free on the root volume, an instance profile for SSM and outbound access to the SSM endpoints
  * It does not handle domain controllers, clusters, desktop editions or Remote Desktop Session Host servers, and the application must be tested before going back to production

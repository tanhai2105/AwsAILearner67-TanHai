---
title: "Week 3 Worklog"
date: 2026-09-21
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Week 3 Objectives:

* Understand how a network is built on AWS with Amazon VPC.
* Get hands-on with Amazon EC2: launching, connecting and changing instances.
* Run a sample application on EC2 inside my own VPC.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - EC2 fundamentals: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + Key pairs <br> - Went over the preparation steps of the EC2 lab | 09/21/2026 | 09/21/2026 | <https://000004.awsfcaj.com/> |
| 3 | - Read the VPC part of the lab: <br>&emsp; + Subnet, Route Table <br>&emsp; + Internet Gateway, NAT Gateway <br> - Read about the two firewall layers: <br>&emsp; + Security Group <br>&emsp; + Network ACL | 09/22/2026 | 09/22/2026 | <https://000003.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + Built a VPC with subnets, an Internet Gateway and a Route Table <br>&emsp; + Added a Security Group <br>&emsp; + Launched EC2 in the VPC and checked connectivity <br>&emsp; + Added a NAT Gateway for the private side | 09/23/2026 | 09/23/2026 | <https://000003.awsfcaj.com/> |
| 5 | - More EC2: <br>&emsp; + Resizing an instance <br>&emsp; + EBS snapshots <br>&emsp; + Building a custom AMI <br> - **Practice:** <br>&emsp; + VPC and Security Groups for the lab <br>&emsp; + Launched a Linux and a Windows instance and logged in to both | 09/24/2026 | 09/24/2026 | <https://000004.awsfcaj.com/> |
| 6 | - How IAM can restrict who launches EC2 and how much it costs <br> - **Practice:** <br>&emsp; + Deployed the AWS User Management app on EC2 <br>&emsp; + Deleted all lab resources | 09/25/2026 | 09/25/2026 | <https://000004.awsfcaj.com/> |


### Week 3 Achievements:

* Know what goes into launching EC2: choosing an instance type and an AMI, attaching EBS storage and keeping the key pair safe.

* Can explain the pieces of a VPC: public and private subnets, how route tables send traffic to an internet gateway or a NAT gateway, and how security groups (stateful, per instance) differ from network ACLs (stateless, per subnet).

* Built a working VPC myself, put an EC2 instance in it and confirmed that the private side reaches the internet only through the NAT gateway.

* Logged in to both a Linux instance and a Windows instance, resized one, took a snapshot and made my own AMI from it.

* Got the AWS User Management application running on EC2, then removed everything so nothing kept charging.

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
| 2 | - Read the EC2 theory: instance types, AMIs, EBS volumes and key pairs <br> - Looked through the preparation part of the EC2 lab | 09/21/2026 | 09/21/2026 | <https://000004.awsfcaj.com/> |
| 3 | - Read the networking part of the VPC lab: subnets, route tables, internet gateways and NAT gateways <br> - Learned the difference between the two firewall layers: security groups and network ACLs | 09/22/2026 | 09/22/2026 | <https://000003.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + Built a VPC with subnets, an internet gateway and a route table, then added a security group <br>&emsp; + Put an EC2 instance in the VPC and tested the connection <br>&emsp; + Attached a NAT gateway so the private side can reach the internet | 09/23/2026 | 09/23/2026 | <https://000003.awsfcaj.com/> |
| 5 | - Kept reading about EC2: resizing an instance, taking EBS snapshots and building a custom AMI <br> - **Practice:** <br>&emsp; + Prepared the VPC and security groups for the lab <br>&emsp; + Ran one Linux and one Windows instance and logged in to both | 09/24/2026 | 09/24/2026 | <https://000004.awsfcaj.com/> |
| 6 | - Studied how IAM can limit who launches EC2, and so keep costs under control <br> - **Practice:** <br>&emsp; + Ran the AWS User Management app on EC2 <br>&emsp; + Cleaned up every lab resource | 09/25/2026 | 09/25/2026 | <https://000004.awsfcaj.com/> |


### Week 3 Achievements:

* Know what goes into launching EC2: choosing an instance type and an AMI, attaching EBS storage and keeping the key pair safe.

* Can explain the pieces of a VPC: public and private subnets, how route tables send traffic to an internet gateway or a NAT gateway, and how security groups (stateful, per instance) differ from network ACLs (stateless, per subnet).

* Built a working VPC myself, put an EC2 instance in it and confirmed that the private side reaches the internet only through the NAT gateway.

* Logged in to both a Linux instance and a Windows instance, resized one, took a snapshot and made my own AMI from it.

* Got the AWS User Management application running on EC2, then removed everything so nothing kept charging.

---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---


### Week 2 Objectives:

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


### Week 2 Achievements:

* Know what has to be chosen when launching EC2: the instance type, the AMI, the EBS volume that comes with it, and a key pair that must be stored carefully.

* Can explain how a VPC is put together: public and private subnets, route tables that lead traffic to an internet gateway or a NAT gateway, and how a security group (stateful, applied to each instance) differs from a network ACL (stateless, applied to the subnet).

* Finished building a usable VPC on my own, placed an EC2 instance in it and confirmed that an instance in the private subnet reaches the internet only via the NAT gateway.

* Logged in to one Linux and one Windows instance, resized one of them, took a snapshot and used it to create an AMI of my own.

* Ran the AWS User Management application on EC2 and deleted all the resources afterwards so there were no further charges.

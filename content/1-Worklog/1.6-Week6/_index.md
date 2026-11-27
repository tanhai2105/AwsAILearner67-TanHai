---
title: "Week 6 Worklog"
date: 2026-10-12
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---


### Week 6 Objectives:

* Deploy two real applications on Amazon Lightsail and keep them safe, backed up and right-sized.
* Put an EC2 application behind Elastic Load Balancing and an Auto Scaling group.
* Try the four scaling approaches and see when each one fits.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Looked at an overview of EC2 Auto Scaling groups and Elastic Load Balancing <br> - **Practice:** <br>&emsp; + Set up the network <br>&emsp; + Created an EC2 instance and a database instance and loaded sample data <br>&emsp; + Installed and started the web server | 10/12/2026 | 10/12/2026 | <https://000006.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Created a launch template, a target group and an Application Load Balancer <br>&emsp; + Opened the app through the load balancer to check it <br>&emsp; + Created the Auto Scaling group and prepared metrics for predictive scaling | 10/13/2026 | 10/13/2026 | <https://000006.awsfcaj.com/> |
| 4 | - **Practice:** <br>&emsp; + Tried all four scaling types in turn: manual, scheduled, dynamic and predictive <br>&emsp; + Cleaned up the lab | 10/14/2026 | 10/14/2026 | <https://000006.awsfcaj.com/> |
| 5 | - Read the Amazon Lightsail overview <br> - **Practice:** <br>&emsp; + Created a managed MySQL database in Lightsail <br>&emsp; + Built a WordPress blog on an Ubuntu blueprint, attached a static IP and then installed WordPress <br>&emsp; + Built a PrestaShop store from its application blueprint, with a static IP and a domain | 10/15/2026 | 10/15/2026 | <https://000045.awsfcaj.com/> |
| 6 | - **Practice:** <br>&emsp; + Restricted the SSH rule of the firewall <br>&emsp; + Took a snapshot, then recreated WordPress from it on a bigger plan <br>&emsp; + Added a CPU alarm <br>&emsp; + Removed the lab resources | 10/16/2026 | 10/16/2026 | <https://000045.awsfcaj.com/> |


### Week 6 Achievements:

* Understood why Auto Scaling matters: unhealthy instances get replaced, capacity grows and shrinks with traffic, the load balancer spreads requests, and quiet hours cost less.

* Built the scaling setup around the FCAJ Management app (EC2 web server plus a database):

  * Launch template with the AMI, instance type, security groups and user data
  * Application Load Balancer with a target group in front of the instances
  * Auto Scaling group over 3 public subnets, desired 1, min 1, max 3, using load balancer health checks, CloudWatch group metrics and SNS notifications

* What I took from the four scaling types:

  * Manual: I change the desired capacity myself for a one-off event
  * Scheduled: capacity changes at set times when traffic follows a known pattern
  * Dynamic: reacts to live metrics such as CPU or request count
  * Predictive: forecasts the next 48 to 72 hours from history, so it needs metric data collected in advance

* Lightsail is the simple option: one monthly price covers the server, SSD, data transfer and a static IP, so I did not have to plan a VPC, security groups or EBS volumes first.

* Brought up two apps on Lightsail in the Singapore Region, each with its own static IP so the address stays fixed:

  * WordPress on an Ubuntu 24.04 blueprint, using a Lightsail managed MySQL database
  * PrestaShop from a ready application blueprint

* Operated them like a small production setup:

  * The SSH rule now only allows the Lightsail browser SSH client rather than the whole internet
  * A Lightsail instance cannot switch plans in place, so I made a snapshot and created a larger instance from it, then pointed the static IP at the new one
  * The blog kept all its posts after the move because the data lives in the managed database, not on the instance
  * Added a CPU alarm

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

* Understood why to use Auto Scaling: failed instances are replaced automatically, the number of instances follows traffic, the load balancer distributes requests, and costs drop when few users are online.

* Built the scaling setup for the FCAJ Management app (a web server on EC2 with a database):

  * A launch template defining the AMI, instance type, security groups and user data
  * An Application Load Balancer and target group placed in front of the instances
  * An Auto Scaling group spread over 3 public subnets with desired 1, min 1, max 3, health checks from the load balancer, CloudWatch group metrics and SNS notifications

* What I learned about the four scaling types:

  * Manual: change the desired capacity by hand for an event that happens once
  * Scheduled: change the number of instances at fixed times when traffic repeats on a pattern
  * Dynamic: react right away to a metric such as CPU or request count
  * Predictive: forecast the next 48–72 hours from historical data, so metrics must have been collected beforehand

* Lightsail is the easy route: one monthly price already covers the server, SSD, data transfer and a static IP, so there is no need to plan a VPC, security groups or EBS up front.

* Ran two applications on Lightsail in the Singapore Region, each with a static IP so the address stays fixed:

  * WordPress on an Ubuntu 24.04 blueprint with a Lightsail-managed MySQL database
  * PrestaShop built from the ready-made application blueprint

* Operated them like a small real-world system:

  * The SSH rule now admits only the Lightsail browser SSH client instead of the whole internet
  * A Lightsail instance cannot change plans directly, so I took a snapshot, built a larger machine from it and pointed the static IP at the new machine
  * The blog kept all its posts after the move, because the data lives in the managed database and not on the instance
  * Added a CPU alarm

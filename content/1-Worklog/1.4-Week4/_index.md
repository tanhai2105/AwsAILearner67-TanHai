---
title: "Week 4 Worklog"
date: 2026-09-28
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Week 4 Objectives:

* Work from the terminal with AWS CloudShell and the AWS CLI.
* Learn how Amazon S3 stores data and how Amazon CloudFront delivers it.
* Host a static site on S3, put CloudFront in front of it and practice object management.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Amazon S3 basics: <br>&emsp; + Buckets and objects <br>&emsp; + Storage classes <br>&emsp; + Versioning and replication <br> - **Practice:** <br>&emsp; + Created a bucket <br>&emsp; + Uploaded the sample website files | 09/28/2026 | 09/28/2026 | <https://000057.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Turned on static website hosting with index.html as the index document <br>&emsp; + Disabled Block public access and set object ACLs to public <br>&emsp; + Loaded the site from the website endpoint and noted how long the page took | 09/29/2026 | 09/29/2026 | <https://000057.awsfcaj.com/> |
| 4 | - Amazon CloudFront basics <br> - **Practice:** <br>&emsp; + Re-enabled Block all public access on the bucket <br>&emsp; + Created a CloudFront distribution with the bucket as origin and updated the bucket policy <br>&emsp; + Loaded the site through CloudFront and compared load time with the S3 endpoint | 09/30/2026 | 09/30/2026 | <https://000057.awsfcaj.com/> |
| 5 | - AWS CloudShell overview <br> - **Practice:** <br>&emsp; + Started a CloudShell environment <br>&emsp; + Tried pwd, ls -la, mkdir, touch, vi and cat <br>&emsp; + Used the AWS CLI to print its version and list my S3 buckets and EC2 instances <br>&emsp; + Uploaded a file to CloudShell and downloaded one back <br>&emsp; + Deleted the environment | 10/01/2026 | 10/01/2026 | <https://000049.awsfcaj.com/> |
| 6 | - Managing objects in S3: <br>&emsp; + Versioning <br>&emsp; + Moving objects <br>&emsp; + Replication across Regions <br> - **Practice:** <br>&emsp; + Enabled versioning, changed index.html, uploaded it again and looked at the version list <br>&emsp; + Moved objects into a second bucket <br>&emsp; + Set up a replication rule to a bucket in N. Virginia <br>&emsp; + Cleaned up the lab resources | 10/02/2026 | 10/02/2026 | <https://000057.awsfcaj.com/> |


### Week 4 Achievements:

* CloudShell turned out to be handy: a ready terminal in the browser with the AWS CLI already logged in. I practiced basic Linux commands there, listed my buckets and instances with the CLI, and moved files in and out.

* Understand that S3 keeps objects inside buckets and that the storage class decides price and access speed. I made a bucket and uploaded the website files into it.

* Got a static website online from S3. To make it reachable I had to turn off Block public access and open the objects with ACLs, which only works over plain HTTP on the website endpoint.

* Then fixed the exposure: switched public access off again and let CloudFront read the bucket through a bucket policy. The page loaded faster via CloudFront, and I could see which edge location (PoP) answered the request.

* Practiced object management:

  * With versioning on, an overwritten index.html could be rolled back to an earlier version
  * Moved objects between buckets
  * A replication rule copied new objects to a bucket in another Region on its own, which works as a backup

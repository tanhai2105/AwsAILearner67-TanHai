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
| 2 | - Learned the basics of Amazon S3: buckets and objects, storage classes, versioning and replication <br> - **Practice:** <br>&emsp; + Created a bucket <br>&emsp; + Uploaded the sample website files | 09/28/2026 | 09/28/2026 | <https://000057.awsfcaj.com/> |
| 3 | - **Practice:** <br>&emsp; + Enabled static website hosting with index.html as the home page <br>&emsp; + Turned off Block public access and made objects public with ACLs <br>&emsp; + Opened the site through the website endpoint and recorded the load time | 09/29/2026 | 09/29/2026 | <https://000057.awsfcaj.com/> |
| 4 | - Read about Amazon CloudFront <br> - **Practice:** <br>&emsp; + Turned Block all public access back on for the bucket <br>&emsp; + Created a CloudFront distribution with the bucket as its origin and updated the bucket policy <br>&emsp; + Opened the site through CloudFront and compared the load time with the S3 website endpoint | 09/30/2026 | 09/30/2026 | <https://000057.awsfcaj.com/> |
| 5 | - Looked at an overview of AWS CloudShell <br> - **Practice:** <br>&emsp; + Opened CloudShell and ran a few Linux commands (pwd, ls -la, mkdir, touch, vi, cat) <br>&emsp; + Used the AWS CLI to check the version and list S3 buckets and EC2 instances <br>&emsp; + Uploaded a file to CloudShell and downloaded one to my computer <br>&emsp; + Deleted the CloudShell environment | 10/01/2026 | 10/01/2026 | <https://000049.awsfcaj.com/> |
| 6 | - Managing objects in S3: versioning, moving objects, replication to another Region <br> - **Practice:** <br>&emsp; + Turned on versioning, edited index.html, uploaded it again and checked the versions <br>&emsp; + Moved objects to a second bucket <br>&emsp; + Created a replication rule to a bucket in N. Virginia <br>&emsp; + Cleaned up the lab resources | 10/02/2026 | 10/02/2026 | <https://000057.awsfcaj.com/> |


### Week 4 Achievements:

* Understand that S3 keeps objects inside buckets and that the storage class decides price and access speed. I made a bucket and uploaded the website files into it.

* Got a static website online from S3. To make it reachable I had to turn off Block public access and open the objects with ACLs, which only works over plain HTTP on the website endpoint.

* Then fixed the exposure: switched public access off again and let CloudFront read the bucket through a bucket policy. The page loaded faster via CloudFront, and I could see which edge location (PoP) answered the request.

* CloudShell turned out to be handy: a ready terminal in the browser with the AWS CLI already logged in. I practiced basic Linux commands there, listed my buckets and instances with the CLI, and moved files in and out.

* Practiced object management:

  * With versioning on, an overwritten index.html could be rolled back to an earlier version
  * Moved objects between buckets
  * A replication rule copied new objects to a bucket in another Region on its own, which works as a backup

---
title: "Blog 2"
date: 2026-11-18
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

### MOVING MICROSOFT SYSTEMS TO AWS: 30% LOWER LICENSE COST AND 70% LOWER STORAGE COST

**Published on:** AWS Study Group (Facebook)

Moving an old .NET Framework and SQL Server system to the cloud with a lift-and-shift approach, meaning it is moved as it is onto new infrastructure, does not solve the problems of license cost and scalability. This post retells a real example from TEKsystems Global Services (TGS): modernizing the whole Microsoft platform of a university into a cloud-native architecture on AWS. The result was 30% lower license cost and 70% lower storage cost.

![Cloud-native architecture on AWS: Route 53, WAF, CloudFront, API Gateway, ECS, Aurora, CloudWatch](../../images/3-BlogsPosted/3.2-Blog2/cloud-native-architecture.png)

**Key points:**

- **Analyze the system before changing it:** the application was a monolith with 785,000 lines of code, 342 tables and 63 stored procedures. Before touching any code, the team ran three automated tools. CAST Highlight finds risks in the source code and barriers to moving to the cloud. CAST Imaging maps the data flow and the dependencies between components, which cut analysis time by 80%. Babelfish Compass checks how well SQL Server (T-SQL) can move to PostgreSQL.
- **Analysis result:** only about 20,000 of the 785,000 lines of code sit in the database layer, under 3% of the application. With the SQL Server dependency that small, replacing it was practical.
- **From monolith to microservices:** AWS Microservices Extractor for .NET analyzes the application and splits it into independent services instead of leaving everything in one block.
- **From .NET Framework to .NET 8:** Porting Assistant and AWS Transform for .NET help convert the source code. After the conversion, the application runs on Linux and AWS Graviton (Arm64).
- **Database and storage:** SQL Server is replaced by PostgreSQL, and Amazon FSx is replaced by Amazon S3. These two changes connect directly to the two cost savings.
- **New architecture based on the AWS Well-Architected Framework:**
  - Amazon ECS runs Linux containers for compute.
  - Amazon ELB and Route 53 handle load balancing and DNS.
  - Amazon S3 and CloudFront deliver content.
  - AWS WAF and Amazon Cognito handle security, and Amazon API Gateway manages the APIs.
  - Amazon CloudWatch monitors the system.
  - AWS Organizations provides central management and audit logs.
- **Cost results:** 30% lower commercial license cost and 70% lower storage cost.

**Conclusion and thoughts:**

The 30% lower license cost and 70% lower storage cost came from analyzing the system first with tools such as CAST and Babelfish Compass, and then moving from .NET Framework to .NET 8 on Linux, from SQL Server to PostgreSQL and from Amazon FSx to Amazon S3. A lift-and-shift move alone would have left both costs where they were.

What I noticed most is the order of the work. The figure of 20,000 lines in the database layer only exists because of the analysis step, and that figure is what told the team that dropping SQL Server was feasible. Without it, the decision to change databases would have been hard to make. The post also shows that cloud modernization is more than moving servers to the cloud: restructuring the application, choosing suitable platforms and tuning the new architecture are what lower cost, improve scalability and make operations more efficient.

**References:**

1. AWS Blog: How TEKsystems modernized a university's Microsoft workloads on AWS — [aws.amazon.com/blogs/modernizing-with-aws](https://aws.amazon.com/blogs/modernizing-with-aws/how-teksystems-modernized-a-universitys-microsoft-workloads-on-aws)
2. AWS Transform for .NET (AWS Documentation, User Guide).
3. AWS Microservices Extractor for .NET (AWS Documentation).
4. Modernize Windows Workloads — [aws.amazon.com/vi/transform/windows](https://aws.amazon.com/vi/transform/windows/)

**Link:** [Post on AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2340011250097167)

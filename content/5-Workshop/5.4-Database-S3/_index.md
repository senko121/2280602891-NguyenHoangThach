---
title : "Create RDS Database and S3 Buckets"
date : 2026-07-13
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Data layer

In this section, you will build the entire data layer of the PetShop system:

+ **Security Groups** — configure a three-tier firewall chain (`ALB → EC2 → RDS`) where each tier only accepts traffic from the tier in front of it, using Security Group references instead of IP addresses.
+ **Amazon RDS (MySQL)** — create the application database in the private database subnets, unreachable from the Internet.
+ **Amazon S3 + IAM** — create two buckets with different access levels: a **public-read images bucket** for product photos, and a **private deployment bucket** for source code and configuration artifacts. An **IAM Role** grants the EC2 instances exactly the permissions they need — no access keys involved.

#### Content

- [Create the Security Group chain](5.4.1-security-groups/)
- [Create the RDS MySQL instance](5.4.2-create-rds/)
- [Create S3 buckets and IAM Role](5.4.3-s3-iam/)

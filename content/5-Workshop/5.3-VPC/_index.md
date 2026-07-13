---
title : "Create VPC network"
date : 2026-07-12
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Network foundation

In this section, you will build the network foundation for the entire PetShop system: a **VPC** spanning **two Availability Zones**, containing **6 subnets** organized into three tiers:

+ **2 Public subnets** — host the NAT Gateway and, later, the Application Load Balancer.
+ **2 Private subnets (app tier)** — host the EC2 instances running the Laravel API. Instances can reach the Internet outbound (to call Stripe, Gmail, Google APIs) through the NAT Gateway, but nothing on the Internet can reach them directly.
+ **2 Private subnets (database tier)** — host the RDS instance, with no route to the Internet at all.

You will also create an **S3 Gateway Endpoint**, allowing instances in the private subnets to upload product images to Amazon S3 through the AWS internal network — free of charge and without consuming NAT Gateway bandwidth.

Instead of creating each component manually, we use the **"VPC and more"** wizard, which provisions the VPC, subnets, route tables, Internet Gateway, NAT Gateway and S3 endpoint in a single step.

![network](/images/5-Workshop/5.3-VPC/network-diagram01.png)

#### Content

- [Create the VPC with the wizard](5.3.1-create-vpc/)
- [Verify and label the network](5.3.2-verify-network/)

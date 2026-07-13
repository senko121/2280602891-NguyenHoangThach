---
title : "Create the Security Group chain"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

The PetShop architecture uses **three Security Groups chained together**: the ALB accepts HTTP from the Internet, the EC2 instances accept HTTP **only from the ALB's Security Group**, and RDS accepts MySQL **only from the EC2 instances' Security Group**. No tier ever opens a port to an IP range wider than necessary.

{{% notice note %}}
Referencing **a Security Group as the traffic source** (instead of an IP/CIDR) means the rule automatically applies to *any* instance attached to that group — even instances launched later by the Auto Scaling Group with unpredictable private IPs.
{{% /notice %}}

1. Open the [EC2 console](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Home:), in the navigation pane choose **Network & Security → Security Groups**, then click **Create security group**.

2. Create the Security Group for the Application Load Balancer:
+ **Security group name**: ```petshop-alb-sg```
+ **Description**: ```ALB public entry```
+ **VPC**: select **petshop-vpc** (⚠️ the console preselects the default VPC — make sure to change it)
+ **Inbound rules**: **Add rule** → Type **HTTP** (80) → Source **Anywhere-IPv4** (`0.0.0.0/0`)
+ Click **Create security group**.

![alb sg](/images/5-Workshop/5.4-Database-S3/sg01.png)

3. Create the Security Group for the EC2 application instances:
+ **Security group name**: ```petshop-app-sg```
+ **Description**: ```EC2 Laravel API```
+ **VPC**: **petshop-vpc**
+ **Inbound rules**: **Add rule** → Type **HTTP** (80) → Source **Custom** → in the search box type ```petshop``` and select **petshop-alb-sg**
+ Click **Create security group**.

![app sg](/images/5-Workshop/5.4-Database-S3/sg02.png)

4. Create the Security Group for RDS:
+ **Security group name**: ```petshop-rds-sg```
+ **Description**: ```MySQL from EC2 app only```
+ **VPC**: **petshop-vpc**
+ **Inbound rules**: **Add rule** → Type **MYSQL/Aurora** (3306) → Source **Custom** → select **petshop-app-sg**
+ Click **Create security group**.

![rds sg](/images/5-Workshop/5.4-Database-S3/sg03.png)

5. Verify the chain: the three Security Groups now form the path **Internet → petshop-alb-sg (80) → petshop-app-sg (80) → petshop-rds-sg (3306)**. Note that `petshop-rds-sg` has **no rule** open to `0.0.0.0/0`.

![sg list](/images/5-Workshop/5.4-Database-S3/sg04.png)

---
title : "Create the RDS MySQL instance"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Create the DB Subnet Group

A DB Subnet Group tells RDS which subnets it is allowed to place the database in — for PetShop, only the two **private database subnets**.

1. Open the [RDS console](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#), in the navigation pane choose **Subnet groups**, then click **Create DB subnet group**:
+ **Name**: ```petshop-db-subnet-group```
+ **Description**: ```Private DB subnets for PetShop```
+ **VPC**: **petshop-vpc**
+ **Availability Zones**: select ```ap-southeast-1a``` and ```ap-southeast-1b```
+ **Subnets**: select exactly the two subnets with CIDR ```10.0.21.0/24``` and ```10.0.22.0/24``` (named `petshop-private-db-1a/1b` in section 5.3.2 — do **not** select the app subnets `10.0.11/12`)
+ Click **Create**.

![subnet group](/images/5-Workshop/5.4-Database-S3/rds01.png)

#### Create the database

2. In the navigation pane choose **Databases**, then click **Create database**:
+ **Choose a database creation method**: **Standard create**
+ **Engine options**: **MySQL**, keep the latest available **MySQL 8.0.x** version
+ **Templates**: **Free tier** (if your account is still Free-Tier eligible; otherwise choose **Dev/Test** with **Single DB instance**)

![engine](/images/5-Workshop/5.4-Database-S3/rds02.png)

3. In the **Settings** section:
+ **DB instance identifier**: ```petshop-db```
+ **Master username**: ```admin```
+ **Credentials management**: **Self managed** — enter a strong password and **write it down**; it goes into `env.aws` as `DB_PASSWORD`

4. **Instance configuration**: ```db.t3.micro```. **Storage**: 20 GiB gp3, and untick **Enable storage autoscaling** to avoid unexpected cost growth.

![settings](/images/5-Workshop/5.4-Database-S3/rds03.png)

5. In the **Connectivity** section (the most error-prone part):
+ **Compute resource**: **Don't connect to an EC2 compute resource**
+ **VPC**: **petshop-vpc**
+ **DB subnet group**: **petshop-db-subnet-group**
+ **Public access**: **No**
+ **VPC security group**: **Choose existing** → remove `default`, select **petshop-rds-sg**
+ **Availability Zone**: ```ap-southeast-1a```

![connectivity](/images/5-Workshop/5.4-Database-S3/rds04.png)

6. Expand **Additional configuration** at the bottom:
+ **Initial database name**: ```petshop``` — ⚠️ required; without it RDS creates an empty instance with no database, and the import in section 5.5.4 will fail
+ Keep automated backups (7 days), untick **Enable Enhanced monitoring**, untick **Deletion protection** (for easy cleanup later)
+ Click **Create database** and wait ~5–10 minutes until **Status = Available**.

{{% notice note %}}
The reference architecture calls for **Multi-AZ** deployment (a standby replica in the second AZ with automatic failover). This workshop runs **Single-AZ** to stay inside the Free Tier — Multi-AZ can be enabled at any time later with **Modify** without recreating the database.
{{% /notice %}}

7. Once available, open the database and copy the **Endpoint** from the **Connectivity & security** tab (e.g. `petshop-db.xxxx.ap-southeast-1.rds.amazonaws.com`). Update `env.aws` on your local machine: set `DB_HOST` to this endpoint and `DB_PASSWORD` to the master password.

![endpoint](/images/5-Workshop/5.4-Database-S3/rds05.png)

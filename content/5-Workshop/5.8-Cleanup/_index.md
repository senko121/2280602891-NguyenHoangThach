---
title : "Clean up"
date : 2026-07-13
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

Congratulations on completing this workshop! 🎉

You deployed a full-stack e-commerce application onto AWS with a production-grade architecture:
+ A **VPC** spanning two Availability Zones with a three-tier subnet layout, NAT Gateway and a free **S3 Gateway Endpoint**.
+ A disposable **EC2 fleet** managed by an Auto Scaling Group behind an Application Load Balancer, bootstrapped entirely from S3 artifacts by a user-data script — no SSH, no manual configuration.
+ **Amazon RDS** in isolated database subnets, reachable only through a chained Security Group.
+ A single **CloudFront** distribution acting as the HTTPS entry point for both the React frontend (private S3 + OAC) and the Laravel API (`/api/*` behavior).

To avoid ongoing charges, delete the resources **in the reverse order of creation** — dependencies must be removed before the things they depend on.

{{% notice warning %}}
💰 The two resources that silently cost money even when idle are the **NAT Gateway (~$32/month)** and the **Application Load Balancer (~$18/month)**. If you only want to pause the project instead of deleting everything, remove at least these two (and release the Elastic IP).
{{% /notice %}}

#### 1. Delete the Auto Scaling Group and compute resources

+ **EC2 → Auto Scaling Groups** → select `petshop-asg` → **Delete** (type `delete` to confirm). This automatically terminates the running instances.
+ **EC2 → Load Balancers** → delete `petshop-alb`.
+ **EC2 → Target Groups** → delete `petshop-api-tg`.
+ **EC2 → Launch Templates** → delete `petshop-lt`.

![delete asg](/images/5-Workshop/5.8-Cleanup/cleanup01.png)

#### 2. Delete the database

+ **RDS → Databases** → select `petshop-db` → **Actions → Delete**.
+ Untick *Create final snapshot*, tick the acknowledgement, type `delete me` to confirm.
+ After the instance is gone, delete the `petshop-db-subnet-group` under **Subnet groups**.

#### 3. Delete the NAT Gateway and release the Elastic IP

+ **VPC → NAT gateways** → select the NAT gateway → **Actions → Delete NAT gateway** (type `delete`).
+ ⚠️ Then go to **VPC → Elastic IPs** → select the released address → **Actions → Release Elastic IP addresses** — an unattached Elastic IP still costs ~$3.6/month.

![delete nat](/images/5-Workshop/5.8-Cleanup/cleanup02.png)

#### 4. Delete the VPC

+ **VPC → Your VPCs** → select `petshop-vpc` → **Actions → Delete VPC**. This removes the subnets, route tables, Internet Gateway, S3 endpoint and security groups in one step (type `delete` to confirm).

#### 5. Empty and delete the S3 buckets

For each of the three buckets (`petshop-frontend-...`, `petshop-images-...`, `petshop-deploy-...`):
+ Select the bucket → **Empty** → type `permanently delete` → confirm.
+ Then select it again → **Delete** → type the bucket name → confirm.

![delete s3](/images/5-Workshop/5.8-Cleanup/cleanup03.png)

#### 6. Delete the CloudFront distribution

+ **CloudFront** → select the distribution → **Disable** and wait until the status change is deployed (a few minutes).
+ Once disabled, select it again → **Delete**.

#### 7. Delete IAM resources

+ **IAM → Roles** → delete `petshop-ec2-role`.
+ **IAM → Policies** → delete `petshop-s3-policy` (and any other `petshop-*` custom policies).

#### 8. Final check

Open **Billing → Bills** (or Cost Explorer) the next day and verify no PetShop-related charges are still accruing — pay special attention to EC2 (NAT Gateway lines), RDS and Elastic IPs.

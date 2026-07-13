---
title : "Create the VPC with the wizard"
date : 2026-07-12
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Open the [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#Home:)
2. Choose **Create VPC**.

![create vpc](/images/5-Workshop/5.3-VPC/vpc01.png)

3. In the **Resources to create** section, select **VPC and more** — the preview diagram on the right will update as you configure each option below.

+ In **Name tag auto-generation**, keep the checkbox ticked and enter: ```petshop```
+ **IPv4 CIDR block**: ```10.0.0.0/16```
+ **IPv6 CIDR block**: **No IPv6 CIDR block**
+ **Tenancy**: **Default**

![create vpc](/images/5-Workshop/5.3-VPC/vpc02.png)

4. Configure Availability Zones and subnets:

+ **Number of Availability Zones (AZs)**: **2** — expand **Customize AZs** and select ```ap-southeast-1a``` and ```ap-southeast-1b```
+ **Number of public subnets**: **2**
+ **Number of private subnets**: **4** (2 for the application tier + 2 for the database tier)
+ Expand **Customize subnets CIDR blocks** and enter the following values:

| Subnet | CIDR |
|---|---|
| Public subnet in ap-southeast-1a | `10.0.1.0/24` |
| Public subnet in ap-southeast-1b | `10.0.2.0/24` |
| Private subnet 1 in ap-southeast-1a (app) | `10.0.11.0/24` |
| Private subnet 1 in ap-southeast-1b (app) | `10.0.12.0/24` |
| Private subnet 2 in ap-southeast-1a (database) | `10.0.21.0/24` |
| Private subnet 2 in ap-southeast-1b (database) | `10.0.22.0/24` |

![create vpc](/images/5-Workshop/5.3-VPC/vpc03.png)

5. Configure NAT Gateway and VPC endpoints:

+ **NAT gateways ($)**: select **In 1 AZ**
+ **VPC endpoints**: select **S3 Gateway**
+ **DNS options**: keep both **Enable DNS hostnames** and **Enable DNS resolution** ticked

{{% notice note %}}
The reference architecture uses **one NAT Gateway per AZ** for high availability. In this workshop we deploy **a single NAT Gateway** to optimize cost (~$32/month each) — this is a common trade-off for non-production environments. The **S3 Gateway Endpoint**, on the other hand, is completely free and keeps image-upload traffic off the NAT Gateway entirely.
{{% /notice %}}

![create vpc](/images/5-Workshop/5.3-VPC/vpc04.png)

6. Choose **Create VPC**. The wizard shows the provisioning progress of every component (VPC, subnets, route tables, Internet Gateway, NAT Gateway, S3 endpoint). The NAT Gateway takes the longest — about 2–3 minutes.

![create vpc](/images/5-Workshop/5.3-VPC/vpc05.png)

7. When all items show a green checkmark, choose **View VPC**.

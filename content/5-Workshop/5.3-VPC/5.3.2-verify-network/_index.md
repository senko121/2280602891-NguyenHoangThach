---
title : "Verify and label the network"
date : 2026-07-12
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

The wizard names the four private subnets generically (`petshop-subnet-private1...4`). Renaming them by tier now prevents choosing the wrong subnet when creating the RDS subnet group (section 5.4) and the Auto Scaling Group (section 5.5).

#### Rename the private subnets

1. In the VPC console navigation pane, choose **Subnets** and filter by the ```petshop-vpc``` VPC.
2. Hover over the **Name** cell of each private subnet, click the pencil icon and rename according to its CIDR:

| CIDR | New name |
|---|---|
| `10.0.11.0/24` | `petshop-private-app-1a` |
| `10.0.12.0/24` | `petshop-private-app-1b` |
| `10.0.21.0/24` | `petshop-private-db-1a` |
| `10.0.22.0/24` | `petshop-private-db-1b` |

![subnets](/images/5-Workshop/5.3-VPC/verify01.png)

#### Verify the route tables

3. In the navigation pane, choose **Route tables**. Select the **public** route table (`petshop-rtb-public`) and open the **Routes** tab — it must contain the route `0.0.0.0/0` pointing to the **Internet Gateway** (`igw-xxxx`):

![public routes](/images/5-Workshop/5.3-VPC/verify02.png)

4. Select one of the **private** route tables and open the **Routes** tab — it must contain **two** important routes:
+ `0.0.0.0/0` → the **NAT Gateway** (`nat-xxxx`) — outbound Internet access for the API instances.
+ A managed prefix list (`pl-xxxx`, Amazon S3) → the **VPC endpoint** (`vpce-xxxx`) — this is the S3 Gateway Endpoint route, keeping S3 traffic on the AWS internal network.

![private routes](/images/5-Workshop/5.3-VPC/verify03.png)

#### Verify the NAT Gateway

The wizard fully configured the NAT Gateway automatically: it allocated an **Elastic IP**, placed the gateway in a **public subnet**, and pointed the private route tables at it (the `nat-xxxx` route seen in the previous step). In this architecture, the NAT Gateway is what allows the API instances in private subnets to make **outbound** calls — installing packages at boot, charging cards through the **Stripe API**, sending emails via **Gmail SMTP**, and verifying **Google login** tokens — while still being unreachable **inbound** from the Internet.

In the navigation pane, choose **NAT gateways** and verify: state **Available**, an Elastic IP assigned, and the subnet is one of the **public** subnets.

![nat gateway](/images/5-Workshop/5.3-VPC/nat01.png)

#### Review the whole network

5. In the navigation pane, choose **Your VPCs**, select ```petshop-vpc``` and open the **Resource map** tab. Verify the complete picture: 6 subnets across 2 AZs, route tables, Internet Gateway, NAT Gateway, and the S3 endpoint — matching the architecture diagram in section 5.1.

![resource map](/images/5-Workshop/5.3-VPC/verify04.png)

{{% notice tip %}}
💰 **Cost-saving tip:** the NAT Gateway is billed hourly even when idle. If you pause the workshop for several days, you can delete the NAT Gateway (and release its Elastic IP), then recreate it later and update the `0.0.0.0/0` route in the private route tables to point to the new NAT.
{{% /notice %}}

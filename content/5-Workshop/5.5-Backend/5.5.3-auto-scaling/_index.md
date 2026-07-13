---
title : "Create the Auto Scaling Group"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

1. In the EC2 console navigation pane, choose **Auto Scaling Groups** → **Create Auto Scaling group**:
+ **Auto Scaling group name**: ```petshop-asg```
+ **Launch template**: select **petshop-lt** → **Next**

2. In **Network**:
+ **VPC**: **petshop-vpc**
+ **Availability Zones and subnets**: select the two **private app** subnets — ```petshop-private-app-1a``` (`10.0.11.0/24`) and ```petshop-private-app-1b``` (`10.0.12.0/24`) → **Next**

![asg network](/images/5-Workshop/5.5-Backend/asg01.png)

3. In **Load balancing**:
+ Select **Attach to an existing load balancer** → **Choose from your load balancer target groups** → select **petshop-api-tg**
+ In *Health checks*, tick **Turn on Elastic Load Balancing health checks** — so the ASG replaces instances that fail the application-level `/api/v1/health` check, not only the basic EC2 status check → **Next**

![asg load balancing](/images/5-Workshop/5.5-Backend/asg02.png)

4. In **Group size and scaling**:
+ **Desired capacity**: ```1``` — **Min**: ```1``` — **Max**: ```2```
+ **Automatic scaling**: select **Target tracking scaling policy**, metric **Average CPU utilization**, target value ```70```

{{% notice note %}}
The reference architecture runs **minimum 2 instances** (one per AZ) for high availability. This workshop runs 1 to stay within the Free Tier — the ASG will still automatically launch a second instance when average CPU exceeds 70%, and replace the instance if it ever becomes unhealthy.
{{% /notice %}}

5. Keep the remaining screens at their defaults → **Create Auto Scaling group**.

6. Wait 3–5 minutes. Verify the deployment:
+ **EC2 → Instances**: a new instance appears, launched by the ASG in a private subnet (it has no public IP).
+ **EC2 → Target Groups → petshop-api-tg → Targets**: the instance status turns **healthy** once the user-data script finishes and nginx starts answering `/api/v1/health`.

![healthy target](/images/5-Workshop/5.5-Backend/asg03.png)

{{% notice tip %}}
If the target stays **unhealthy** for more than ~5 minutes, connect to the instance with Session Manager (section 5.5.4) and inspect the boot log: ```cat /var/log/user-data.log``` — the last lines show exactly which step failed (usually a wrong bucket name in the user-data script).
{{% /notice %}}

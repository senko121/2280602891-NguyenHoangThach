---
title : "Create the Target Group and ALB"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

#### Create the Target Group

1. Open the [EC2 console](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Home:), in the navigation pane choose **Target Groups**, then click **Create target group**:
+ **Choose a target type**: **Instances**
+ **Target group name**: ```petshop-api-tg```
+ **Protocol : Port**: **HTTP : 80**
+ **VPC**: **petshop-vpc**
+ In **Health checks**, set **Health check path** to: ```/api/v1/health```

{{% notice note %}}
`/api/v1/health` is a dedicated route in the Laravel application that returns `{"status":"ok"}` without touching the database or requiring authentication. The ALB calls it every 30 seconds on every instance — any instance that stops answering is marked *unhealthy* and automatically replaced by the Auto Scaling Group.
{{% /notice %}}

![target group](/images/5-Workshop/5.5-Backend/alb01.png)

2. Click **Next**. On the *Register targets* screen, do **not** register any instance — the Auto Scaling Group will register them automatically in section 5.5.3. Click **Create target group**.

#### Create the Application Load Balancer

3. In the navigation pane choose **Load Balancers** → **Create load balancer** → under *Application Load Balancer* click **Create**:
+ **Load balancer name**: ```petshop-alb```
+ **Scheme**: **Internet-facing**
+ **IP address type**: **IPv4**

4. In **Network mapping**:
+ **VPC**: **petshop-vpc**
+ Tick both Availability Zones and, for each, select the **public** subnet (`10.0.1.0/24` and `10.0.2.0/24`) — ⚠️ do not select the private subnets.

![network mapping](/images/5-Workshop/5.5-Backend/alb02.png)

5. In **Security groups**: remove `default`, select **petshop-alb-sg**.
6. In **Listeners and routing**: keep **HTTP : 80** and set **Default action** to forward to **petshop-api-tg**.
7. Click **Create load balancer** and wait until **State = Active**.

8. Open the ALB details and copy the **DNS name** (e.g. `petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com`).

![alb dns](/images/5-Workshop/5.5-Backend/alb03.png)

9. Update `env.aws` on your local machine — set `APP_URL` to this DNS name:

```
APP_URL=http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com
```

Then re-upload `env.aws` to the **petshop-deploy** bucket (S3 console → bucket → **Upload**, overwriting the old file). The instances read this file from S3 at boot, so the bucket copy must always be the latest version.

---
title : "Deploy Backend with EC2, ALB and Auto Scaling"
date : 2026-07-13
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Application layer

In this section, you will bring the Laravel API to life on AWS:

+ **Target Group & Application Load Balancer** — the public entry point of the API, performing health checks against the application's `/api/v1/health` endpoint.
+ **Launch Template** — a blueprint describing how every API instance is built: Amazon Linux 2023, nginx + PHP 8.2, and a **user-data script** that automatically downloads the source code and configuration from the private S3 bucket at boot time.
+ **Auto Scaling Group** — keeps the desired number of instances alive across the two private app subnets, replaces unhealthy instances automatically, and scales out when CPU utilization is high.
+ **Database import** — using **Session Manager** to reach the private instances from the browser, import the SQL dump into RDS and verify the API end-to-end.

Because instances are created **from a template + artifacts stored in S3**, the fleet is fully disposable: any instance can be terminated and an identical replacement boots up with zero manual configuration — the core idea of immutable infrastructure.

#### Content

- [Create the Target Group and ALB](5.5.1-alb-target-group/)
- [Create the Launch Template](5.5.2-launch-template/)
- [Create the Auto Scaling Group](5.5.3-auto-scaling/)
- [Import the database and verify the API](5.5.4-import-database/)

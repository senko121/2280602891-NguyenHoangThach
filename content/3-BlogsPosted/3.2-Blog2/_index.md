---
title: "Blog 2"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# OPTIMIZING NAT GATEWAY COSTS ON AWS

Hello everyone 👋

If you've worked with AWS and deployed systems inside a VPC, you've almost certainly heard of **NAT Gateway**.

### What is a NAT Gateway?

When we place EC2 instances or applications inside a **Private Subnet**, they don't have a Public IP, so they can't reach the Internet directly. However, they often still need outbound access — to update the operating system, download libraries from npm or Maven, call third-party APIs, pull Docker images, or connect to other external services.

This is exactly where **NAT Gateway** comes in. It's a fully managed AWS service that lets private resources initiate outbound Internet connections safely.

### The problem: NAT Gateway can burn through your budget

AWS charges for NAT Gateway based on three main factors:

* **Uptime** — the NAT Gateway runs 24/7 whether or not there's traffic.
* **Data processed** — billed per GB.
* **Data transfer fees** — incurred when data crosses different AZs.

**Example:**

Suppose your system spans **3 Availability Zones**. AZ1 has 1 NAT Gateway plus 1,000 GB of traffic per month. AZ2 and AZ3 are the same.

The cost for each AZ is calculated as follows:

* Hourly fee: 0.045 × 730 hours = **$32.85**.
* Data processing fee: 0.045 × 1,000 GB = **$45**.
* **Total per AZ: $77.85/month.**

Combined cost across three AZs: 77.85 × 3 = **$233.55/month**.

So is there any way to bring this number down?

### Solution 1: Identify the traffic

Before optimizing, you need to know where the data is actually going. Questions to answer include:

* Which EC2 instance generates the most traffic?
* Does the traffic go out to the Internet, or is it only reaching AWS services?
* How much traffic goes to S3?
* Are there any unused NAT Gateways?

Tools for this analysis include:

* **Amazon CloudWatch** to track metrics such as `BytesInFromSource`, `BytesOutToDestination`, and `ConnectionAttemptCount`.
* **VPC Flow Logs** to log detailed traffic in and out of the VPC.
* **Amazon S3** to store the VPC Flow Logs.
* **Amazon Athena** to query the logs with SQL and find the "top talkers".

**Example analysis:** after analyzing VPC Flow Logs, you find that **50% of traffic goes to Amazon S3**, 30% calls external APIs, 15% downloads packages and updates, and 5% goes to other services.

### Solution 2: Use an S3 Gateway Endpoint

In the example above, 50% of traffic goes to S3. This data doesn't necessarily need to pass through the NAT Gateway.

**Without an S3 Endpoint:** Private EC2 → NAT Gateway → Internet → S3.

**With an S3 Endpoint:** Private EC2 → S3 Gateway Endpoint → S3, entirely within AWS's internal network — no NAT Gateway or public Internet involved.

**Cost after adding the S3 Endpoint:** assuming 50% of traffic no longer needs the NAT Gateway, each AZ is left with 500 GB.

* NAT hourly fee: **$32.85**.
* Processing fee for 500 GB: 0.045 × 500 = **$22.50**.
* **Total per AZ: $55.35/month.**

Total for three AZs: 55.35 × 3 = **$166.05/month**.

Compared to three NAT Gateways without an endpoint ($233.55/month), this saves **$67.50/month**.

![AWS NAT Gateway Cost Optimization](/images/3-Blog/aws_NAT.png)

**Reference:**

https://vegacloud.medium.com/aws-nat-gateway-optimization-bd5f7d2da8a8

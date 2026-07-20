---
title: "Proposal"
date: "2026-05-12"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you need to summarize the proposed solution for deploying the PetCare Shop platform on AWS.

# PetCare Shop Platform
## A Scalable AWS Architecture for Pet Shop Management and Pet Care Booking

### 1. Executive Summary

PetCare Shop is a cloud-based web application designed for pet store management and pet care service booking. The platform allows customers to browse products, place orders, and schedule pet care appointments, while administrators can manage products, bookings, and customer information through a centralized dashboard. The system is deployed on AWS using highly available and scalable infrastructure to improve performance, security, and operational reliability.

---

## 2. Problem Statement

### What's the Problem?

Many small pet shops still rely on manual management or standalone applications that cannot efficiently handle product sales, appointment scheduling, and customer management simultaneously. In addition, deploying applications on a single server creates a single point of failure, making the system difficult to scale and maintain.

### The Solution

The proposed solution deploys the PetCare Shop platform on AWS using a multi-tier architecture. Users access the application through Amazon Route 53, Amazon CloudFront, and AWS WAF before requests are distributed by an Application Load Balancer to Amazon EC2 instances running inside an Auto Scaling Group. Application data is stored in Amazon RDS, while product images and uploaded files are stored in Amazon S3. Amazon CloudWatch continuously monitors system performance and application logs.

### Benefits and Return on Investment

The AWS architecture improves application availability, security, and scalability while reducing maintenance effort. Auto Scaling automatically adjusts computing resources based on traffic, CloudFront improves website performance through content caching, and Amazon S3 reduces storage usage on EC2 by storing static files separately. Although the monthly infrastructure cost is higher than traditional shared hosting, the architecture provides a production-ready environment that can easily support future business growth.

---

## 3. Solution Architecture

The proposed architecture follows AWS best practices for web applications. Incoming requests are resolved by Amazon Route 53 and accelerated through Amazon CloudFront. AWS WAF filters malicious traffic before requests reach the Application Load Balancer. The load balancer distributes requests to multiple EC2 instances deployed inside private subnets and managed by an Auto Scaling Group. Application data is stored in Amazon RDS, while product images are uploaded directly to Amazon S3. Private EC2 instances access external services through a NAT Gateway, and Amazon CloudWatch provides centralized monitoring and logging.

![PetCare Shop Architecture](/images/2-Proposal/petcare_architecture.png)

### AWS Services Used

- **Amazon Route 53** – DNS management.
- **Amazon CloudFront** – Content delivery and caching.
- **AWS WAF** – Protects against common web attacks.
- **Application Load Balancer** – Distributes incoming traffic.
- **Amazon EC2** – Hosts the backend application.
- **Amazon EC2 Auto Scaling** – Automatically scales EC2 instances.
- **Amazon RDS** – Stores application data.
- **Amazon S3** – Stores frontend files and product images.
- **NAT Gateway** – Provides Internet access for private EC2 instances.
- **Amazon CloudWatch** – Monitors infrastructure and application logs.

### Component Design

- **Frontend:** Hosted on Amazon S3 and delivered through CloudFront.
- **Backend:** Spring Boot application running on EC2.
- **Database:** Amazon RDS MySQL.
- **Storage:** Amazon S3 for images and static files.
- **Load Balancing:** Application Load Balancer.
- **Scaling:** EC2 Auto Scaling Group.
- **Monitoring:** Amazon CloudWatch.
- **Security:** AWS WAF and private subnets with NAT Gateway.

---

## 4. Technical Implementation

### Implementation Phases

The project follows four implementation phases:

- Design the AWS architecture and estimate infrastructure costs.
- Deploy networking, security, and compute resources.
- Develop and deploy the frontend and backend applications.
- Test, optimize, and monitor the production environment.

### Technical Requirements

- **Frontend:** ReactJS
- **Backend:** Spring Boot REST API
- **Database:** MySQL on Amazon RDS
- **Cloud Infrastructure:** EC2, ALB, Auto Scaling Group, Route 53, CloudFront, AWS WAF, Amazon S3, NAT Gateway, CloudWatch

---

## 5. Timeline & Milestones

### Project Timeline

- **Month 1:** System analysis and AWS architecture design.
- **Month 2:** Infrastructure deployment and application development.
- **Month 3:** Testing, optimization, and production deployment.

---

## 6. Budget Estimation

The infrastructure cost is estimated using the AWS Pricing Calculator based on the Singapore (ap-southeast-1) Region. The system follows an **elastic cost model**: actual spend depends on the current load and availability tier the system is running at, rather than a single fixed number.

### Tier 1 — Normal Operating Cost

The baseline cost while the system runs at normal load: the Auto Scaling Group stays at Desired Capacity (1 EC2), with a single NAT Gateway and a Single-AZ RDS instance.

| Service | Cost/month |
|---|---|
| Amazon EC2 (1 × t3.micro) | ~$15.00 |
| Amazon RDS MySQL Single-AZ (db.t3.micro) | ~$18.00 |
| Application Load Balancer | ~$18.00 |
| Amazon CloudFront | ~$5.00 |
| Amazon S3 (Frontend) | ~$0.10 |
| Amazon S3 (Images) | ~$1.00 |
| Amazon Route 53 | ~$0.50 |
| Amazon CloudWatch | ~$3.00 |
| NAT Gateway (1 gateway) | ~$50.00 |
| **Total (Tier 1)** | **\~$110.60/month (\~$1,327/year)** |

### Tier 2 — Under High Traffic (Auto Scaling triggered)

Under increased load, the Auto Scaling Group scales out to Maximum Capacity (2 EC2), and the NAT Gateway processes more data — cost increases in line with actual traffic.

| Cause of increase | Additional cost |
|---|---|
| 2nd EC2 launched by Auto Scaling (Max = 2) | +$15.00 |
| Extra NAT Gateway data processing from increased traffic | +$15.00 |
| **Total (Tier 1 + Tier 2)** | **~$140.60/month** |

### Tier 3 — Upgrading to Full Production HA

This is the cost of deploying the full high-availability reference architecture: adding a second NAT Gateway to remove the cross-AZ single point of failure, enabling AWS WAF, and optionally upgrading RDS to Multi-AZ.

| Additional upgrade | Additional cost |
|---|---|
| 2nd NAT Gateway (per-AZ redundancy) | +$50.00 |
| AWS WAF | +$7.00 |
| *(Optional)* RDS Multi-AZ | +$18.00 |
| **Total (Tier 1 + Tier 3, excluding RDS Multi-AZ)** | **~$167.60/month** |
| **Total (Tier 1 + Tier 3, including RDS Multi-AZ)** | **~$185.60/month** |

*Note: The estimated cost is based on the AWS Pricing Calculator for the Singapore (ap-southeast-1) Region and may vary depending on actual usage, data transfer, and AWS pricing changes. The hands-on workshop in this report is deployed at **Tier 1** to optimize learning costs; the architecture can be upgraded to Tier 3 when moved into a real production environment.*

---

## 7. Risk Assessment

### Risk Matrix

- EC2 failure – Medium impact, low probability.
- Database failure – High impact, low probability.
- Traffic spikes – Medium impact, medium probability.
- Security attacks – High impact, medium probability.

### Mitigation Strategies

- Auto Scaling automatically replaces unhealthy instances.
- Amazon RDS automated backups protect application data.
- AWS WAF filters malicious requests.
- CloudWatch alarms notify administrators of abnormal system behavior.

### Contingency Plans

- Restore the database from automated backups.
- Launch replacement EC2 instances through Auto Scaling.
- Scale resources according to application traffic.

---

## 8. Expected Outcomes

### Technical Improvements

Deploy a highly available, secure, and scalable cloud architecture capable of supporting online shopping and pet care booking services.

### Long-term Value

The proposed infrastructure provides a solid foundation for future enhancements such as online payment integration, CI/CD pipelines, containerization with Amazon ECS, and AI-based recommendation systems.
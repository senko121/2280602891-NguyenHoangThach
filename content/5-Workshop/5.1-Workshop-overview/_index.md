---
title : "Introduction"
date : 2026-07-12
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### The PetShop application

+ **PetShop** is a full-stack web application for a pet store, combining an **e-commerce shop** (pet food, toys, accessories) with a **pet care booking system** (grooming, bathing, pet hotel, pet rescue). Customers can browse products, add them to a cart, book service time slots, and pay for everything in a single checkout using **Stripe** or **VNPay**.
+ The application consists of two independent codebases: a **React 18** single-page application (SPA) for the user interface, and a **Laravel 10** REST API for business logic, authentication (Laravel Sanctum + Google OAuth), order management, and email notifications. This separation allows each tier to be deployed, scaled, and secured independently on AWS.

#### Workshop overview

In this workshop, you will deploy the PetShop application onto AWS using a production-grade architecture spanning **two Availability Zones**:

+ **Amazon CloudFront** acts as the single secure entry point of the system. It serves the React frontend from a private **Amazon S3** bucket (via Origin Access Control), and forwards all requests matching `/api/*` to the backend through a second origin, giving the whole application HTTPS without purchasing a custom domain.
+ The **Laravel API** runs on **Amazon EC2** instances placed in **private subnets**, fully isolated from the Internet. An **Application Load Balancer** in the public subnets distributes traffic to the instances, and an **Auto Scaling Group** automatically replaces unhealthy instances and scales out when CPU utilization is high.
+ **Amazon RDS (MySQL)** stores all application data (products, services, orders, bookings, users) in dedicated private database subnets, only reachable from the application instances through a chained Security Group rule.
+ **Amazon S3** stores all product and service images in a separate bucket. The EC2 instances upload images to this bucket through an **S3 Gateway Endpoint**, so image traffic never traverses the NAT Gateway or the public Internet.
+ **AWS Systems Manager Session Manager** is used to administer the private EC2 instances directly from the browser, eliminating the need for SSH key pairs or bastion hosts.

#### AWS services used in this workshop

| Service | Role in the architecture |
|---|---|
| Amazon VPC | Network foundation: 6 subnets across 2 AZs, Internet Gateway, NAT Gateway, S3 Gateway Endpoint |
| Amazon EC2 + Auto Scaling | Runs the Laravel API (Amazon Linux 2023, nginx + PHP 8.2) |
| Application Load Balancer | Distributes API traffic, health checks via `/api/v1/health` |
| Amazon RDS (MySQL 8.0) | Application database |
| Amazon S3 | 3 buckets: frontend hosting, product images, deployment artifacts |
| Amazon CloudFront | Global CDN + single HTTPS entry point for both frontend and API |
| AWS IAM | EC2 instance role for S3 access and Session Manager |
| AWS Systems Manager | Browser-based shell access to private instances |

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram01.png)

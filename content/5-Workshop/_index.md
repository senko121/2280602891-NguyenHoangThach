---
title: "Workshop"
date: 2026-07-12
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying a Full-Stack Pet Shop Web Application on AWS

#### Overview

**PetShop** is a full-stack e-commerce and pet care booking web application, built with **React** (frontend) and **Laravel** (backend API), allowing customers to purchase pet products and book pet care services such as grooming, bathing, and pet hotel.

In this lab, you will learn how to deploy the entire PetShop application onto AWS following a production-grade, highly available architecture: the React frontend is served globally through **Amazon CloudFront** from a private **Amazon S3** bucket, while the Laravel API runs on **Amazon EC2** instances inside private subnets, managed by an **Auto Scaling Group** behind an **Application Load Balancer**, with data stored in **Amazon RDS (MySQL)** and product images stored in **Amazon S3**.

You will build the infrastructure layer by layer, and each layer offers a different benefit depending on the role it plays in the overall architecture:
+ **Network layer** - Create a VPC spanning two Availability Zones with public and private subnets, NAT Gateway, and an S3 Gateway Endpoint so that private workloads can reach Amazon S3 without traversing the Public Internet.
+ **Application layer** - Deploy the Laravel API on EC2 instances in private subnets behind an Application Load Balancer, and serve the React frontend through CloudFront with a single distribution acting as the secure entry point for both the website and the API.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Create VPC network](5.3-VPC/)
4. [Create RDS Database and S3 Buckets](5.4-Database-S3/)
5. [Deploy Backend with EC2, ALB and Auto Scaling](5.5-Backend/)
6. [Deploy Frontend with S3 and CloudFront](5.6-Frontend/)
7. [Test the application](5.7-Testing/)
8. [Clean up](5.8-Cleanup/)

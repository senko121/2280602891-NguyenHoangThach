---
title : "Deploy Frontend with S3 and CloudFront"
date : 2026-07-13
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Presentation layer

In this section, you will deploy the React frontend and connect it to the backend — using one clever architectural decision: **a single CloudFront distribution serves both the website and the API**.

+ The **default behavior** serves the React static files from a **private S3 bucket** through **Origin Access Control (OAC)** — the bucket is never exposed to the Internet.
+ A **second behavior** matching ```/api/*``` forwards API requests to the **Application Load Balancer** origin.

This design gives the whole application **HTTPS on the free `*.cloudfront.net` domain** without purchasing a custom domain or an ACM certificate, and it avoids the classic **mixed-content problem** (an HTTPS page is not allowed to call an HTTP API — with a single distribution, both the page and the API share the same HTTPS origin).

#### Content

- [Create the frontend bucket and CloudFront distribution](5.6.1-cloudfront-s3/)
- [Add the API origin and behavior](5.6.2-api-behavior/)
- [Build, upload and connect everything](5.6.3-build-deploy/)

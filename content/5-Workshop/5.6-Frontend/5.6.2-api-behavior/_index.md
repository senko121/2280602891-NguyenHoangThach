---
title : "Add the API origin and behavior"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

The distribution currently serves only static files from S3. Now teach it to forward every request whose path starts with `/api/` to the Application Load Balancer.

#### Add the ALB origin

1. Open the distribution → **Origins** tab → **Create origin**:
+ **Origin domain**: paste the **ALB DNS name** (`petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com`)
+ **Protocol**: **HTTP only** — the ALB has no HTTPS certificate; the connection viewer↔CloudFront remains HTTPS
+ Click **Create origin**.

![alb origin](/images/5-Workshop/5.6-Frontend/cf05.png)

#### Create the /api/* behavior

2. Open the **Behaviors** tab → **Create behavior**:
+ **Path pattern**: ```/api/*```
+ **Origin**: select the ALB origin created in step 1
+ **Viewer protocol policy**: **Redirect HTTP to HTTPS**
+ **Allowed HTTP methods**: **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE** — the API needs all of them (login is POST, order status update is PUT, image removal is DELETE...)
+ **Cache policy**: **CachingDisabled** — API responses are dynamic and must never be cached
+ **Origin request policy**: **AllViewer** — forwards all headers (including `Authorization` bearer tokens), cookies and query strings to the backend
+ Click **Create behavior**.

![api behavior](/images/5-Workshop/5.6-Frontend/cf06.png)

{{% notice warning %}}
⚠️ Three settings on this screen silently break the application when missed: **all 7 HTTP methods** (otherwise login/checkout fail), **CachingDisabled** (otherwise one user's cart may be served to another user from cache), and **AllViewer** (otherwise the `Authorization` header is stripped and every authenticated call returns 401).
{{% /notice %}}

3. Verify the **Behaviors** tab now lists two entries, with ```/api/*``` ranked **above** ```Default (*)``` — CloudFront evaluates the most specific path first.

![behaviors list](/images/5-Workshop/5.6-Frontend/cf07.png)

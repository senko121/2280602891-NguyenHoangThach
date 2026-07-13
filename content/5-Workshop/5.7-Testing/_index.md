---
title : "Test the application"
date : 2026-07-13
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

The whole system is live — time to verify the critical user flows end-to-end. Every test below runs in the browser against ```https://<your-cloudfront-domain>```.

#### 1. Home page and images

Open the website: the home page must load over **HTTPS**, and product images must display — they are served from the **S3 images bucket** (right-click an image → *Open image in new tab* to confirm the `s3.ap-southeast-1.amazonaws.com` URL).

#### 2. Authentication

+ Register a new account, then log in with it.
+ Log in with **Google** — this confirms the *Authorized JavaScript origins* configuration from section 5.6.3.

![login](/images/5-Workshop/5.7-Testing/test01.png)

#### 3. Place an order with Stripe

Add a product to the cart and check out using the Stripe **test card**:

```
Card number : 4242 4242 4242 4242
Expiry      : any future date
CVC         : any 3 digits
```

The order must complete and redirect to the thank-you page. This proves the full chain: **CloudFront → ALB → EC2 → Stripe API (via NAT Gateway) → RDS**.

![stripe order](/images/5-Workshop/5.7-Testing/test02.png)

#### 4. Order tracking

Open **My Orders**: the new order appears with its status tracker, and the order data is read back from **RDS**.

![my orders](/images/5-Workshop/5.7-Testing/test03.png)

#### 5. SPA refresh

While standing on any sub-page (e.g. a product detail page), press **F5**. The page must reload correctly instead of showing an S3 error — this verifies the custom error responses from section 5.6.1.

#### 6. Admin image upload

Log in as an administrator and create a new product with an image. After saving:
+ The product image displays on the website.
+ The image file appears in the **petshop-images** bucket (`product/` folder) — uploaded by the EC2 instance through the **S3 Gateway Endpoint** using its **IAM Role**.

![admin upload](/images/5-Workshop/5.7-Testing/test04.png)

{{% notice tip %}}
If any step fails, check in this order: browser **F12 → Console/Network** for the failing request → CloudFront behavior settings (section 5.6.2) → `sudo tail /var/www/petshop/storage/logs/laravel.log` on the instance via Session Manager.
{{% /notice %}}

All six checks passing means the PetShop application is fully operational on AWS. 🎉

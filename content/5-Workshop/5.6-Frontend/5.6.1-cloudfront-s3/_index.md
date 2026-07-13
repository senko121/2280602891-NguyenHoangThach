---
title : "Create the frontend bucket and CloudFront distribution"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

#### Create the private frontend bucket

1. Open the [S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1) → **Create bucket**:
+ **Bucket name**: ```petshop-frontend-<your-unique-name>```
+ **Block Public Access settings**: **keep "Block all public access" ticked** ✅ — unlike the images bucket, nobody accesses this bucket directly; only CloudFront reads it through OAC.
+ Click **Create bucket**.

#### Create the CloudFront distribution

2. Open the [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home) → **Create distribution**:
+ **Origin domain**: select the ```petshop-frontend-...``` bucket from the dropdown
+ **Origin access**: select **Origin access control settings (recommended)** → click **Create new OAC** → keep the defaults → **Create**

![origin](/images/5-Workshop/5.6-Frontend/cf01.png)

3. In **Default cache behavior**:
+ **Viewer protocol policy**: **Redirect HTTP to HTTPS**
4. **Web Application Firewall (WAF)**: select **Do not enable security protections** (WAF costs ~$8+/month; it can be enabled later with one click).
5. In **Settings**, set **Default root object**: ```index.html``` → click **Create distribution**.

6. Right after creation, a blue banner appears: *"Complete distribution configuration by allowing CloudFront to access the S3 bucket"* — click **Copy policy**, then open the S3 bucket → **Permissions** → **Bucket policy** → **Edit** → paste → **Save changes**. This policy allows **only this CloudFront distribution** to read the bucket.

![bucket policy](/images/5-Workshop/5.6-Frontend/cf02.png)

#### Configure error responses for the React SPA

7. The React application is a Single-Page Application: routes like `/collections/dog-food` exist only in the browser, not as files in S3. When a visitor refreshes such a page, S3 returns an error — CloudFront must be told to serve `index.html` instead and let React Router take over.

Open the distribution → **Error pages** tab → **Create custom error response**, and create **two** rules:

| Setting | Rule 1 | Rule 2 |
|---|---|---|
| HTTP error code | **403: Forbidden** | **404: Not Found** |
| Customize error response | **Yes** | **Yes** |
| Response page path | ```/index.html``` | ```/index.html``` |
| HTTP response code | **200: OK** | **200: OK** |

![error pages](/images/5-Workshop/5.6-Frontend/cf03.png)

8. From the **General** tab, copy the **Distribution domain name** (e.g. `d1a2b3c4.cloudfront.net`) — this is the public address of the PetShop website.

![domain](/images/5-Workshop/5.6-Frontend/cf04.png)

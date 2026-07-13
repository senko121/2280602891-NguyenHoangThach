---
title : "Create S3 buckets and IAM Role"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Create the images bucket (public read)

1. Open the [S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1), click **Create bucket**:
+ **Bucket name**: ```petshop-images-<your-unique-name>``` (bucket names are globally unique — add your own suffix). Write this name down: it is the `AWS_BUCKET` value in `env.aws` and part of `REACT_APP_IMAGE_URL` in section 5.6.
+ **AWS Region**: ```ap-southeast-1```
+ **Block Public Access settings**: **untick** "Block *all* public access" and tick the yellow acknowledgement box — website visitors must be able to load product images directly from this bucket.
+ Keep everything else default, click **Create bucket**.

![images bucket](/images/5-Workshop/5.4-Database-S3/s301.png)

2. Open the bucket → **Permissions** tab → **Bucket policy** → **Edit**, and paste the policy below (replace the bucket name in `Resource`). It grants the public **read-only** access (`s3:GetObject`) — only the EC2 role created later can write or delete:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadImages",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::petshop-images-<your-unique-name>/*"
        }
    ]
}
```

![bucket policy](/images/5-Workshop/5.4-Database-S3/s302.png)

3. Upload the existing product images: click **Upload → Add folder** and add the four folders from the local project (`Laravel-api/storage/app/public/`): `product`, `category`, `service`, `album`. After uploading, the four folders must sit **at the root of the bucket**.

4. Test: open any image file in the bucket, copy its **Object URL** and open it in a browser tab — the image must display. If you get `AccessDenied`, re-check the bucket policy in step 2.

![uploaded](/images/5-Workshop/5.4-Database-S3/s303.png)

#### Create the deployment bucket (private)

5. Create a second bucket:
+ **Bucket name**: ```petshop-deploy-<your-unique-name>```
+ **Block Public Access settings**: **keep "Block all public access" ticked** ✅ — this bucket holds source code and secrets.
6. Upload the three artifacts prepared in section 5.2 into the bucket root: `petshop-api.zip`, `petshop_dump.sql`, and `env.aws`.

![deploy bucket](/images/5-Workshop/5.4-Database-S3/s304.png)

{{% notice warning %}}
⚠️ Never place `env.aws` or the source code zip in the **images** bucket — its policy makes every object publicly readable, which would expose your database password and API secrets to the Internet.
{{% /notice %}}

#### Create the IAM Role for EC2

7. Open the [IAM console](https://us-east-1.console.aws.amazon.com/iam/home#/policies) → **Policies** → **Create policy** → **JSON** tab, paste the policy below (replace both bucket names). Name it ```petshop-s3-policy```:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ImagesReadWrite",
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
            "Resource": "arn:aws:s3:::petshop-images-<your-unique-name>/*"
        },
        {
            "Sid": "ImagesList",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::petshop-images-<your-unique-name>"
        },
        {
            "Sid": "DeployReadOnly",
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:ListBucket"],
            "Resource": [
                "arn:aws:s3:::petshop-deploy-<your-unique-name>",
                "arn:aws:s3:::petshop-deploy-<your-unique-name>/*"
            ]
        }
    ]
}
```

![iam policy](/images/5-Workshop/5.4-Database-S3/iam01.png)

8. In the navigation pane choose **Roles** → **Create role**:
+ **Trusted entity type**: **AWS service** — **Use case**: **EC2**
+ On the permissions screen, attach **two** policies: ```petshop-s3-policy``` (created above) and ```AmazonSSMManagedInstanceCore``` (AWS managed — enables browser-based Session Manager access, replacing SSH keys and bastion hosts)
+ **Role name**: ```petshop-ec2-role``` → **Create role**.

![iam role](/images/5-Workshop/5.4-Database-S3/iam02.png)

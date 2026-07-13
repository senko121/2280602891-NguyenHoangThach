---
title : "Prerequiste"
date : 2026-07-12
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### AWS account and region

+ An AWS account with administrator access (or sufficient permissions for VPC, EC2, RDS, S3, CloudFront, IAM and Systems Manager).
+ In this lab, we will use the **Singapore region (ap-southeast-1)**, which is closest to Vietnam. Make sure the region selector at the top-right corner of the AWS Console shows **Asia Pacific (Singapore)** before creating any resource.

![region](/images/5-Workshop/5.2-Prerequisite/region01.png)

#### Application source code

The PetShop project consists of two folders:

```
PetShop/
├── Laravel-api/        # Laravel 10 REST API (backend)
└── react-project/      # React 18 single-page application (frontend)
```

![project](/images/5-Workshop/5.2-Prerequisite/project-structure.png)

#### Prepare deployment artifacts

Before touching the AWS Console, prepare **three artifacts** on your local machine. They will be uploaded to a private S3 bucket in section 5.4, then pulled down automatically by the EC2 instances at boot time.

1. **Package the backend source code** (including the `vendor/` folder, so the instances do not need to run Composer). Exclude the local `.env` file and cache folders:

```powershell
cd PetShop
tar -a -cf petshop-api.zip --exclude=".env" --exclude="storage/logs/*.log" `
    --exclude="public/storage" --exclude="storage/framework/cache/data" `
    --exclude="storage/framework/sessions" --exclude="storage/framework/views" `
    -C Laravel-api .
```

2. **Export the local MySQL database**. Use `--result-file` (instead of shell redirection) so that Vietnamese UTF-8 characters are preserved correctly on Windows:

```powershell
mysqldump -u root -p -h 127.0.0.1 --single-transaction --routines --triggers `
    --set-gtid-purged=OFF --default-character-set=utf8mb4 `
    --result-file="petshop_dump.sql" petshop
```

3. **Create the production environment file** `env.aws` — a copy of the Laravel `.env` with production values. The placeholders below will be filled in as the AWS resources are created in the next sections:

```
APP_ENV=production
APP_DEBUG=false
APP_URL=http://<ALB-DNS-name>                  # filled in section 5.5
FRONTEND_URL=https://<CloudFront-domain>       # filled in section 5.6
CORS_ALLOWED_ORIGINS=https://<CloudFront-domain>

DB_CONNECTION=mysql
DB_HOST=<RDS-endpoint>                         # filled in section 5.4
DB_PORT=3306
DB_DATABASE=petshop
DB_USERNAME=admin
DB_PASSWORD=<your-RDS-password>

FILESYSTEM_DISK=s3
AWS_ACCESS_KEY_ID=                             # leave empty - EC2 uses an IAM Role
AWS_SECRET_ACCESS_KEY=                         # leave empty - EC2 uses an IAM Role
AWS_DEFAULT_REGION=ap-southeast-1
AWS_BUCKET=<your-images-bucket-name>           # filled in section 5.4

MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=<your-gmail>
MAIL_PASSWORD=<gmail-app-password>
MAIL_ENCRYPTION=tls

STRIPE_KEY=<stripe-publishable-key>
STRIPE_SECRET=<stripe-secret-key>
GOOGLE_CLIENT_ID=<google-oauth-client-id>
GOOGLE_CLIENT_SECRET=<google-oauth-client-secret>
```

{{% notice note %}}
Because the EC2 instances are granted an **IAM Role** (created in section 5.4), the two `AWS_ACCESS_KEY_*` variables are intentionally left **empty** — the AWS SDK automatically obtains temporary credentials from the instance role. Never hard-code long-lived access keys in configuration files.
{{% /notice %}}

After this step, the three artifacts are ready on the local machine:

![artifacts](/images/5-Workshop/5.2-Prerequisite/artifacts.png)

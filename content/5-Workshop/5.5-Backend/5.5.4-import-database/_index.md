---
title : "Import the database and verify the API"
date : 2026-07-13
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

#### Connect to the private instance with Session Manager

1. In the EC2 console, choose **Instances**, select the instance launched by the ASG, click **Connect** → open the **Session Manager** tab → **Connect**. A shell opens right in the browser — no SSH key, no bastion host, no open port 22.

![session manager](/images/5-Workshop/5.5-Backend/ssm01.png)

#### Import the SQL dump into RDS

2. In the session, run the following commands (replace `<your-deploy-bucket>` and `<RDS-endpoint>`; enter the RDS master password when prompted):

```bash
sudo -s
aws s3 cp s3://<your-deploy-bucket>/petshop_dump.sql /tmp/
mysql -h <RDS-endpoint> -u admin -p petshop < /tmp/petshop_dump.sql
```

3. Verify the import — the tables and sample data must be present:

```bash
mysql -h <RDS-endpoint> -u admin -p petshop \
    -e "SHOW TABLES; SELECT COUNT(*) AS products FROM products;"
```

![import](/images/5-Workshop/5.5-Backend/ssm02.png)

{{% notice warning %}}
⚠️ Never run `php artisan config:cache` on these servers. The application reads several variables (Stripe keys, VNPay credentials) with `env()` at runtime — once a config cache exists, Laravel stops reading `.env` entirely and every `env()` call returns `null`, silently breaking the payment gateway. If it was ever run by mistake, fix it with `php artisan config:clear`.
{{% /notice %}}

#### Verify the API through the ALB

4. From your own browser, open the two URLs below (replace with your ALB DNS name):

+ ```http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com/api/v1/health``` → must return `{"status":"ok"}`
+ ```http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com/api/v1/viewHomePage``` → must return a JSON payload full of products and services imported from the dump

![api test](/images/5-Workshop/5.5-Backend/api01.png)

The backend is now fully operational on AWS: **Internet → ALB (public subnets) → EC2 (private subnets) → RDS (database subnets) + S3 (images)**. The next section puts the React frontend in front of it.

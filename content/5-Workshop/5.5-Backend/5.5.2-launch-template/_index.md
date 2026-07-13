---
title : "Create the Launch Template"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

1. In the EC2 console navigation pane, choose **Launch Templates** → **Create launch template**:
+ **Launch template name**: ```petshop-lt```
+ **Application and OS Images**: *Quick Start* → **Amazon Linux 2023 AMI** (64-bit x86)
+ **Instance type**: ```t3.micro```
+ **Key pair**: **Proceed without a key pair** — administration is done through Session Manager, no SSH needed

![launch template](/images/5-Workshop/5.5-Backend/lt01.png)

2. In **Network settings**:
+ **Subnet**: **Don't include in launch template** (the Auto Scaling Group chooses the subnets)
+ **Security groups**: select **petshop-app-sg**

3. Expand **Advanced details**:
+ **IAM instance profile**: select **petshop-ec2-role** — this is how instances get S3 and Session Manager permissions without any access key

![advanced details](/images/5-Workshop/5.5-Backend/lt02.png)

4. Scroll to the bottom of *Advanced details* and paste the script below into the **User data** field. ⚠️ Replace `<your-deploy-bucket>` on the `DEPLOY_BUCKET` line with your actual deployment bucket name:

```bash
#!/bin/bash
set -e
exec > /var/log/user-data.log 2>&1

DEPLOY_BUCKET="<your-deploy-bucket>"

echo "=== 1. Install packages ==="
dnf install -y nginx php8.2 php8.2-fpm php8.2-cli php8.2-mysqlnd php8.2-mbstring \
    php8.2-xml php8.2-gd php8.2-opcache php8.2-bcmath php8.2-intl mariadb105 unzip

echo "=== 2. Download code + .env from S3 ==="
mkdir -p /var/www/petshop
aws s3 cp "s3://${DEPLOY_BUCKET}/petshop-api.zip" /tmp/petshop-api.zip
unzip -oq /tmp/petshop-api.zip -d /var/www/petshop
aws s3 cp "s3://${DEPLOY_BUCKET}/env.aws" /var/www/petshop/.env
rm -f /tmp/petshop-api.zip

echo "=== 3. Laravel folder permissions ==="
cd /var/www/petshop
mkdir -p storage/framework/cache/data storage/framework/sessions storage/framework/views storage/logs
chown -R nginx:nginx /var/www/petshop
chmod -R 775 storage bootstrap/cache

echo "=== 4. Run PHP-FPM as the nginx user ==="
sed -i 's/^user = apache/user = nginx/; s/^group = apache/group = nginx/' /etc/php-fpm.d/www.conf

echo "=== 5. Configure nginx ==="
cat > /etc/nginx/nginx.conf <<'NGINX'
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

events { worker_connections 1024; }

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    access_log /var/log/nginx/access.log;
    sendfile on;
    keepalive_timeout 65;

    server {
        listen 80 default_server;
        server_name _;
        root /var/www/petshop/public;
        index index.php;
        client_max_body_size 20m;

        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
            fastcgi_pass unix:/run/php-fpm/www.sock;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
        }
    }
}
NGINX

echo "=== 6. Start services ==="
systemctl enable --now php-fpm nginx

echo "=== DONE === $(date)"
```

![user data](/images/5-Workshop/5.5-Backend/lt03.png)

{{% notice note %}}
The script runs **once, automatically, at first boot** of every instance. It installs nginx + PHP 8.2, pulls the packaged source code and `env.aws` from the private S3 bucket (authorized by the IAM role — no credentials in the script), fixes file ownership, points PHP-FPM and nginx at the Laravel `public/` folder, and starts the services. Full execution takes about 2–3 minutes; its log is written to `/var/log/user-data.log` for troubleshooting.
{{% /notice %}}

5. Click **Create launch template**.

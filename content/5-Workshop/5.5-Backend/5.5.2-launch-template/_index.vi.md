---
title : "Tạo Launch Template"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

1. Ở thanh điều hướng EC2 console, chọn **Launch Templates** → **Create launch template**:
+ **Launch template name**: ```petshop-lt```
+ **Application and OS Images**: *Quick Start* → **Amazon Linux 2023 AMI** (64-bit x86)
+ **Instance type**: ```t3.micro```
+ **Key pair**: **Proceed without a key pair** — việc quản trị thực hiện qua Session Manager, không cần SSH

![launch template](/images/5-Workshop/5.5-Backend/lt01.png)

2. Ở mục **Network settings**:
+ **Subnet**: **Don't include in launch template** (Auto Scaling Group sẽ chọn subnet)
+ **Security groups**: chọn **petshop-app-sg**

3. Mở **Advanced details**:
+ **IAM instance profile**: chọn **petshop-ec2-role** — đây là cách instance có quyền S3 và Session Manager mà không cần bất kỳ access key nào

![advanced details](/images/5-Workshop/5.5-Backend/lt02.png)

4. Kéo xuống cuối *Advanced details* và dán script bên dưới vào ô **User data**. ⚠️ Thay `<ten-bucket-deploy>` ở dòng `DEPLOY_BUCKET` bằng tên bucket triển khai thật của bạn:

```bash
#!/bin/bash
set -e
exec > /var/log/user-data.log 2>&1

DEPLOY_BUCKET="<ten-bucket-deploy>"

echo "=== 1. Cai dat phan mem ==="
dnf install -y nginx php8.2 php8.2-fpm php8.2-cli php8.2-mysqlnd php8.2-mbstring \
    php8.2-xml php8.2-gd php8.2-opcache php8.2-bcmath php8.2-intl mariadb105 unzip

echo "=== 2. Tai code + .env tu S3 ==="
mkdir -p /var/www/petshop
aws s3 cp "s3://${DEPLOY_BUCKET}/petshop-api.zip" /tmp/petshop-api.zip
unzip -oq /tmp/petshop-api.zip -d /var/www/petshop
aws s3 cp "s3://${DEPLOY_BUCKET}/env.aws" /var/www/petshop/.env
rm -f /tmp/petshop-api.zip

echo "=== 3. Quyen thu muc Laravel ==="
cd /var/www/petshop
mkdir -p storage/framework/cache/data storage/framework/sessions storage/framework/views storage/logs
chown -R nginx:nginx /var/www/petshop
chmod -R 775 storage bootstrap/cache

echo "=== 4. PHP-FPM chay bang user nginx ==="
sed -i 's/^user = apache/user = nginx/; s/^group = apache/group = nginx/' /etc/php-fpm.d/www.conf

echo "=== 5. Cau hinh nginx ==="
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

echo "=== 6. Khoi dong dich vu ==="
systemctl enable --now php-fpm nginx

echo "=== HOAN TAT === $(date)"
```

![user data](/images/5-Workshop/5.5-Backend/lt03.png)

{{% notice note %}}
Script này chạy **một lần, tự động, ngay lần khởi động đầu tiên** của mỗi instance. Nó cài nginx + PHP 8.2, kéo mã nguồn đã đóng gói và file `env.aws` từ S3 bucket riêng tư (được cấp quyền qua IAM Role — không có credentials nào trong script), sửa quyền sở hữu file, trỏ PHP-FPM và nginx vào thư mục `public/` của Laravel, rồi khởi động dịch vụ. Toàn bộ mất khoảng 2–3 phút; log được ghi tại `/var/log/user-data.log` để tiện chẩn đoán khi có lỗi.
{{% /notice %}}

5. Bấm **Create launch template**.

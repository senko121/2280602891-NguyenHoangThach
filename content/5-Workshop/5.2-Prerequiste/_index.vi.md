---
title : "Các bước chuẩn bị"
date : 2026-07-12
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### Tài khoản AWS và Region

+ Một tài khoản AWS có quyền quản trị (hoặc đủ quyền với các dịch vụ VPC, EC2, RDS, S3, CloudFront, IAM và Systems Manager).
+ Trong workshop này, chúng ta sử dụng **region Singapore (ap-southeast-1)** — region gần Việt Nam nhất. Hãy chắc chắn bộ chọn region ở góc trên bên phải AWS Console đang hiển thị **Asia Pacific (Singapore)** trước khi tạo bất kỳ tài nguyên nào.

![region](/images/5-Workshop/5.2-Prerequisite/region01.png)

#### Mã nguồn ứng dụng

Dự án PetShop gồm hai thư mục:

```
PetShop/
├── Laravel-api/        # Laravel 10 REST API (backend)
└── react-project/      # React 18 single-page application (frontend)
```

![project](/images/5-Workshop/5.2-Prerequisite/project-structure.png)

#### Chuẩn bị artifacts triển khai

Trước khi thao tác trên AWS Console, cần chuẩn bị **ba artifacts** trên máy cá nhân. Chúng sẽ được tải lên một S3 bucket riêng tư ở mục 5.4, sau đó các EC2 instance tự động tải xuống khi khởi động.

1. **Đóng gói mã nguồn backend** (bao gồm cả thư mục `vendor/` để instance không cần chạy Composer). Loại trừ file `.env` local và các thư mục cache:

```powershell
cd PetShop
tar -a -cf petshop-api.zip --exclude=".env" --exclude="storage/logs/*.log" `
    --exclude="public/storage" --exclude="storage/framework/cache/data" `
    --exclude="storage/framework/sessions" --exclude="storage/framework/views" `
    -C Laravel-api .
```

2. **Xuất cơ sở dữ liệu MySQL local**. Dùng `--result-file` (thay vì redirect qua shell) để giữ nguyên ký tự tiếng Việt UTF-8 trên Windows:

```powershell
mysqldump -u root -p -h 127.0.0.1 --single-transaction --routines --triggers `
    --set-gtid-purged=OFF --default-character-set=utf8mb4 `
    --result-file="petshop_dump.sql" petshop
```

3. **Tạo file môi trường production** `env.aws` — bản sao của file `.env` Laravel với các giá trị production. Các giá trị placeholder bên dưới sẽ được điền dần khi tài nguyên AWS được tạo ở các mục tiếp theo:

```
APP_ENV=production
APP_DEBUG=false
APP_URL=http://<DNS-cua-ALB>                   # điền ở mục 5.5
FRONTEND_URL=https://<domain-CloudFront>       # điền ở mục 5.6
CORS_ALLOWED_ORIGINS=https://<domain-CloudFront>

DB_CONNECTION=mysql
DB_HOST=<endpoint-RDS>                         # điền ở mục 5.4
DB_PORT=3306
DB_DATABASE=petshop
DB_USERNAME=admin
DB_PASSWORD=<mat-khau-RDS>

FILESYSTEM_DISK=s3
AWS_ACCESS_KEY_ID=                             # để trống - EC2 dùng IAM Role
AWS_SECRET_ACCESS_KEY=                         # để trống - EC2 dùng IAM Role
AWS_DEFAULT_REGION=ap-southeast-1
AWS_BUCKET=<ten-bucket-anh>                    # điền ở mục 5.4

MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=<gmail-cua-ban>
MAIL_PASSWORD=<app-password-gmail>
MAIL_ENCRYPTION=tls

STRIPE_KEY=<stripe-publishable-key>
STRIPE_SECRET=<stripe-secret-key>
GOOGLE_CLIENT_ID=<google-oauth-client-id>
GOOGLE_CLIENT_SECRET=<google-oauth-client-secret>
```

{{% notice note %}}
Vì các EC2 instance được gắn **IAM Role** (tạo ở mục 5.4), hai biến `AWS_ACCESS_KEY_*` được cố ý để **trống** — AWS SDK sẽ tự động lấy thông tin xác thực tạm thời từ role của instance. Không bao giờ hard-code access key dài hạn trong file cấu hình.
{{% /notice %}}

Sau bước này, ba artifacts đã sẵn sàng trên máy cá nhân:

![artifacts](/images/5-Workshop/5.2-Prerequisite/artifacts.png)

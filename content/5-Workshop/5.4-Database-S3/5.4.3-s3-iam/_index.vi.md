---
title : "Tạo S3 bucket và IAM Role"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Tạo bucket ảnh (đọc công khai)

1. Mở [S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1), bấm **Create bucket**:
+ **Bucket name**: ```petshop-images-<ten-rieng-cua-ban>``` (tên bucket là duy nhất toàn cầu — thêm hậu tố riêng của bạn). Ghi lại tên này: nó là giá trị `AWS_BUCKET` trong `env.aws` và nằm trong `REACT_APP_IMAGE_URL` ở mục 5.6.
+ **AWS Region**: ```ap-southeast-1```
+ **Block Public Access settings**: **bỏ tick** "Block *all* public access" và tick vào ô xác nhận màu vàng — khách truy cập website phải tải được ảnh sản phẩm trực tiếp từ bucket này.
+ Các mục còn lại giữ mặc định, bấm **Create bucket**.

![images bucket](/images/5-Workshop/5.4-Database-S3/s301.png)

2. Mở bucket → tab **Permissions** → **Bucket policy** → **Edit**, dán policy bên dưới (thay tên bucket ở dòng `Resource`). Policy này chỉ cấp quyền **đọc** (`s3:GetObject`) cho public — chỉ IAM Role của EC2 tạo ở phần sau mới ghi/xóa được:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadImages",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::petshop-images-<ten-rieng-cua-ban>/*"
        }
    ]
}
```

![bucket policy](/images/5-Workshop/5.4-Database-S3/s302.png)

3. Upload ảnh sản phẩm hiện có: bấm **Upload → Add folder** và thêm bốn thư mục từ project local (`Laravel-api/storage/app/public/`): `product`, `category`, `service`, `album`. Sau khi upload, bốn thư mục phải nằm **ngay gốc bucket**.

4. Kiểm tra: mở một file ảnh bất kỳ trong bucket, copy **Object URL** và mở trong tab trình duyệt — ảnh phải hiển thị. Nếu gặp `AccessDenied`, kiểm tra lại bucket policy ở bước 2.

![uploaded](/images/5-Workshop/5.4-Database-S3/s303.png)

#### Tạo bucket triển khai (riêng tư)

5. Tạo bucket thứ hai:
+ **Bucket name**: ```petshop-deploy-<ten-rieng-cua-ban>```
+ **Block Public Access settings**: **giữ nguyên tick "Block all public access"** ✅ — bucket này chứa mã nguồn và secrets.
6. Upload ba artifacts đã chuẩn bị ở mục 5.2 vào gốc bucket: `petshop-api.zip`, `petshop_dump.sql` và `env.aws`.

![deploy bucket](/images/5-Workshop/5.4-Database-S3/s304.png)

{{% notice warning %}}
⚠️ Tuyệt đối không đặt `env.aws` hay file zip mã nguồn vào bucket **ảnh** — policy của nó cho phép đọc công khai mọi object, đồng nghĩa mật khẩu database và API secrets sẽ bị lộ ra Internet.
{{% /notice %}}

#### Tạo IAM Role cho EC2

7. Mở [IAM console](https://us-east-1.console.aws.amazon.com/iam/home#/policies) → **Policies** → **Create policy** → tab **JSON**, dán policy bên dưới (thay cả hai tên bucket). Đặt tên ```petshop-s3-policy```:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ImagesReadWrite",
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
            "Resource": "arn:aws:s3:::petshop-images-<ten-rieng-cua-ban>/*"
        },
        {
            "Sid": "ImagesList",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::petshop-images-<ten-rieng-cua-ban>"
        },
        {
            "Sid": "DeployReadOnly",
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:ListBucket"],
            "Resource": [
                "arn:aws:s3:::petshop-deploy-<ten-rieng-cua-ban>",
                "arn:aws:s3:::petshop-deploy-<ten-rieng-cua-ban>/*"
            ]
        }
    ]
}
```

![iam policy](/images/5-Workshop/5.4-Database-S3/iam01.png)

8. Ở thanh điều hướng chọn **Roles** → **Create role**:
+ **Trusted entity type**: **AWS service** — **Use case**: **EC2**
+ Ở màn hình chọn quyền, gắn **hai** policy: ```petshop-s3-policy``` (vừa tạo) và ```AmazonSSMManagedInstanceCore``` (policy có sẵn của AWS — cho phép truy cập Session Manager qua trình duyệt, thay thế SSH key và bastion host)
+ **Role name**: ```petshop-ec2-role``` → **Create role**.

![iam role](/images/5-Workshop/5.4-Database-S3/iam02.png)

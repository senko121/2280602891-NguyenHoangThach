---
title : "Tạo cơ sở dữ liệu RDS và S3 Bucket"
date : 2026-07-13
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tầng dữ liệu

Trong phần này, bạn sẽ xây dựng toàn bộ tầng dữ liệu của hệ thống PetShop:

+ **Security Group** — cấu hình chuỗi tường lửa ba lớp (`ALB → EC2 → RDS`), trong đó mỗi lớp chỉ chấp nhận lưu lượng từ lớp đứng trước nó, sử dụng kỹ thuật tham chiếu Security Group thay vì địa chỉ IP.
+ **Amazon RDS (MySQL)** — tạo cơ sở dữ liệu ứng dụng trong các Private Subnet dành riêng cho database, không thể truy cập từ Internet.
+ **Amazon S3 + IAM** — tạo hai bucket với mức truy cập khác nhau: **bucket ảnh cho phép đọc công khai** chứa hình sản phẩm, và **bucket triển khai riêng tư** chứa mã nguồn cùng file cấu hình. Một **IAM Role** cấp cho các EC2 instance đúng những quyền chúng cần — hoàn toàn không dùng access key.

#### Nội dung

- [Tạo chuỗi Security Group](5.4.1-security-groups/)
- [Tạo RDS MySQL instance](5.4.2-create-rds/)
- [Tạo S3 bucket và IAM Role](5.4.3-s3-iam/)

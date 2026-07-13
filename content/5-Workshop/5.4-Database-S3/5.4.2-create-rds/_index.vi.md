---
title : "Tạo RDS MySQL instance"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Tạo DB Subnet Group

DB Subnet Group cho RDS biết nó được phép đặt database vào những subnet nào — với PetShop, chỉ là hai **Private Subnet dành cho database**.

1. Mở [RDS console](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#), ở thanh điều hướng chọn **Subnet groups**, sau đó bấm **Create DB subnet group**:
+ **Name**: ```petshop-db-subnet-group```
+ **Description**: ```Private DB subnets for PetShop```
+ **VPC**: **petshop-vpc**
+ **Availability Zones**: chọn ```ap-southeast-1a``` và ```ap-southeast-1b```
+ **Subnets**: chọn đúng hai subnet có CIDR ```10.0.21.0/24``` và ```10.0.22.0/24``` (đã đặt tên `petshop-private-db-1a/1b` ở mục 5.3.2 — **không** chọn nhầm subnet app `10.0.11/12`)
+ Bấm **Create**.

![subnet group](/images/5-Workshop/5.4-Database-S3/rds01.png)

#### Tạo database

2. Ở thanh điều hướng chọn **Databases**, sau đó bấm **Create database**:
+ **Choose a database creation method**: **Standard create**
+ **Engine options**: **MySQL**, giữ phiên bản **MySQL 8.0.x** mới nhất có sẵn
+ **Templates**: **Free tier** (nếu tài khoản còn Free Tier; nếu không thì chọn **Dev/Test** với **Single DB instance**)

![engine](/images/5-Workshop/5.4-Database-S3/rds02.png)

3. Ở mục **Settings**:
+ **DB instance identifier**: ```petshop-db```
+ **Master username**: ```admin```
+ **Credentials management**: **Self managed** — nhập mật khẩu mạnh và **ghi lại ngay**; giá trị này sẽ điền vào `env.aws` ở biến `DB_PASSWORD`

4. **Instance configuration**: ```db.t3.micro```. **Storage**: 20 GiB gp3, và bỏ tick **Enable storage autoscaling** để tránh chi phí tăng ngoài ý muốn.

![settings](/images/5-Workshop/5.4-Database-S3/rds03.png)

5. Ở mục **Connectivity** (phần dễ sai nhất):
+ **Compute resource**: **Don't connect to an EC2 compute resource**
+ **VPC**: **petshop-vpc**
+ **DB subnet group**: **petshop-db-subnet-group**
+ **Public access**: **No**
+ **VPC security group**: **Choose existing** → bỏ `default`, chọn **petshop-rds-sg**
+ **Availability Zone**: ```ap-southeast-1a```

![connectivity](/images/5-Workshop/5.4-Database-S3/rds04.png)

6. Mở **Additional configuration** ở cuối trang:
+ **Initial database name**: ```petshop``` — ⚠️ bắt buộc điền; nếu bỏ trống, RDS tạo instance rỗng không có database nào và bước import ở mục 5.5.4 sẽ thất bại
+ Giữ automated backups (7 ngày), bỏ tick **Enable Enhanced monitoring**, bỏ tick **Deletion protection** (để dễ dọn dẹp sau này)
+ Bấm **Create database** và chờ ~5–10 phút đến khi **Status = Available**.

{{% notice note %}}
Kiến trúc tham chiếu yêu cầu triển khai **Multi-AZ** (bản standby ở AZ thứ hai với failover tự động). Workshop này chạy **Single-AZ** để nằm trong Free Tier — có thể bật Multi-AZ bất cứ lúc nào sau này bằng nút **Modify** mà không cần tạo lại database.
{{% /notice %}}

7. Khi database đã Available, mở nó lên và copy **Endpoint** ở tab **Connectivity & security** (dạng `petshop-db.xxxx.ap-southeast-1.rds.amazonaws.com`). Cập nhật file `env.aws` trên máy cá nhân: điền endpoint này vào `DB_HOST` và mật khẩu master vào `DB_PASSWORD`.

![endpoint](/images/5-Workshop/5.4-Database-S3/rds05.png)

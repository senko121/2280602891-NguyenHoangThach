---
title : "Tạo chuỗi Security Group"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Kiến trúc PetShop sử dụng **ba Security Group nối chuỗi với nhau**: ALB nhận HTTP từ Internet, các EC2 instance chỉ nhận HTTP **từ Security Group của ALB**, và RDS chỉ nhận MySQL **từ Security Group của các EC2 instance**. Không tầng nào mở cổng cho dải IP rộng hơn mức cần thiết.

{{% notice note %}}
Việc dùng **Security Group làm nguồn lưu lượng** (thay vì IP/CIDR) nghĩa là quy tắc tự động áp dụng cho *mọi* instance gắn group đó — kể cả các instance được Auto Scaling Group tạo ra sau này với IP private không thể đoán trước.
{{% /notice %}}

1. Mở [EC2 console](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Home:), ở thanh điều hướng chọn **Network & Security → Security Groups**, sau đó bấm **Create security group**.

2. Tạo Security Group cho Application Load Balancer:
+ **Security group name**: ```petshop-alb-sg```
+ **Description**: ```ALB public entry```
+ **VPC**: chọn **petshop-vpc** (⚠️ console mặc định chọn sẵn VPC default — nhớ đổi lại)
+ **Inbound rules**: **Add rule** → Type **HTTP** (80) → Source **Anywhere-IPv4** (`0.0.0.0/0`)
+ Bấm **Create security group**.

![alb sg](/images/5-Workshop/5.4-Database-S3/sg01.png)

3. Tạo Security Group cho các EC2 instance ứng dụng:
+ **Security group name**: ```petshop-app-sg```
+ **Description**: ```EC2 Laravel API```
+ **VPC**: **petshop-vpc**
+ **Inbound rules**: **Add rule** → Type **HTTP** (80) → Source **Custom** → gõ ```petshop``` vào ô tìm kiếm và chọn **petshop-alb-sg**
+ Bấm **Create security group**.

![app sg](/images/5-Workshop/5.4-Database-S3/sg02.png)

4. Tạo Security Group cho RDS:
+ **Security group name**: ```petshop-rds-sg```
+ **Description**: ```MySQL from EC2 app only```
+ **VPC**: **petshop-vpc**
+ **Inbound rules**: **Add rule** → Type **MYSQL/Aurora** (3306) → Source **Custom** → chọn **petshop-app-sg**
+ Bấm **Create security group**.

![rds sg](/images/5-Workshop/5.4-Database-S3/sg03.png)

5. Kiểm tra lại chuỗi: ba Security Group giờ tạo thành đường đi **Internet → petshop-alb-sg (80) → petshop-app-sg (80) → petshop-rds-sg (3306)**. Lưu ý `petshop-rds-sg` **không có rule nào** mở cho `0.0.0.0/0`.

![sg list](/images/5-Workshop/5.4-Database-S3/sg04.png)

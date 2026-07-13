---
title : "Tạo VPC bằng wizard"
date : 2026-07-12
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#Home:)
2. Chọn **Create VPC**.

![create vpc](/images/5-Workshop/5.3-VPC/vpc01.png)

3. Ở mục **Resources to create**, chọn **VPC and more** — sơ đồ preview bên phải sẽ tự cập nhật theo từng lựa chọn bên dưới.

+ Ở **Name tag auto-generation**, giữ nguyên dấu tick và nhập: ```petshop```
+ **IPv4 CIDR block**: ```10.0.0.0/16```
+ **IPv6 CIDR block**: **No IPv6 CIDR block**
+ **Tenancy**: **Default**

![create vpc](/images/5-Workshop/5.3-VPC/vpc02.png)

4. Cấu hình Availability Zone và subnet:

+ **Number of Availability Zones (AZs)**: **2** — mở **Customize AZs** và chọn ```ap-southeast-1a``` và ```ap-southeast-1b```
+ **Number of public subnets**: **2**
+ **Number of private subnets**: **4** (2 cho tầng ứng dụng + 2 cho tầng database)
+ Mở **Customize subnets CIDR blocks** và nhập các giá trị sau:

| Subnet | CIDR |
|---|---|
| Public subnet tại ap-southeast-1a | `10.0.1.0/24` |
| Public subnet tại ap-southeast-1b | `10.0.2.0/24` |
| Private subnet 1 tại ap-southeast-1a (app) | `10.0.11.0/24` |
| Private subnet 1 tại ap-southeast-1b (app) | `10.0.12.0/24` |
| Private subnet 2 tại ap-southeast-1a (database) | `10.0.21.0/24` |
| Private subnet 2 tại ap-southeast-1b (database) | `10.0.22.0/24` |

![create vpc](/images/5-Workshop/5.3-VPC/vpc03.png)

5. Cấu hình NAT Gateway và VPC endpoint:

+ **NAT gateways ($)**: chọn **In 1 AZ**
+ **VPC endpoints**: chọn **S3 Gateway**
+ **DNS options**: giữ nguyên cả hai dấu tick **Enable DNS hostnames** và **Enable DNS resolution**

{{% notice note %}}
Kiến trúc tham chiếu sử dụng **một NAT Gateway cho mỗi AZ** để đạt tính sẵn sàng cao. Trong workshop này, chúng ta triển khai **một NAT Gateway duy nhất** để tối ưu chi phí (~$32/tháng mỗi chiếc) — đây là đánh đổi phổ biến cho môi trường phi production. Ngược lại, **S3 Gateway Endpoint** hoàn toàn miễn phí và giúp toàn bộ lưu lượng upload ảnh không đi qua NAT Gateway.
{{% /notice %}}

![create vpc](/images/5-Workshop/5.3-VPC/vpc04.png)

6. Chọn **Create VPC**. Wizard sẽ hiển thị tiến trình tạo từng thành phần (VPC, subnet, route table, Internet Gateway, NAT Gateway, S3 endpoint). NAT Gateway mất nhiều thời gian nhất — khoảng 2–3 phút.

![create vpc](/images/5-Workshop/5.3-VPC/vpc05.png)

7. Khi tất cả các mục hiện dấu tick xanh, chọn **View VPC**.

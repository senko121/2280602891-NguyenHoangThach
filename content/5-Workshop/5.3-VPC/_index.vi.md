---
title : "Tạo hạ tầng mạng VPC"
date : 2026-07-12
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Nền tảng mạng

Trong phần này, bạn sẽ xây dựng nền tảng mạng cho toàn bộ hệ thống PetShop: một **VPC** trải rộng trên **hai Availability Zone**, chứa **6 subnet** được tổ chức thành ba tầng:

+ **2 Public subnet** — chứa NAT Gateway và sau này là Application Load Balancer.
+ **2 Private subnet (tầng ứng dụng)** — chứa các EC2 instance chạy API Laravel. Instance có thể đi ra Internet (để gọi Stripe, Gmail, Google API) thông qua NAT Gateway, nhưng không gì từ Internet có thể truy cập trực tiếp vào chúng.
+ **2 Private subnet (tầng database)** — chứa RDS instance, hoàn toàn không có đường ra Internet.

Bạn cũng sẽ tạo một **S3 Gateway Endpoint**, cho phép các instance trong Private Subnet tải ảnh sản phẩm lên Amazon S3 qua mạng nội bộ của AWS — miễn phí và không tiêu tốn băng thông NAT Gateway.

Thay vì tạo thủ công từng thành phần, chúng ta dùng wizard **"VPC and more"** — công cụ tự động tạo VPC, subnet, route table, Internet Gateway, NAT Gateway và S3 endpoint chỉ trong một bước.

![network](/images/5-Workshop/5.3-VPC/network-diagram01.png)

#### Nội dung

- [Tạo VPC bằng wizard](5.3.1-create-vpc/)
- [Kiểm tra và đặt tên mạng](5.3.2-verify-network/)

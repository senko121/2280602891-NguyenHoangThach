---
title: "Workshop"
date: 2026-07-12
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai ứng dụng PetShop Full-Stack trên AWS

#### Tổng quan

**PetShop** là một ứng dụng web thương mại điện tử kết hợp đặt lịch chăm sóc thú cưng, được phát triển theo mô hình **Full-Stack** với **React** (Frontend) và **Laravel** (Backend API). Ứng dụng cho phép khách hàng mua các sản phẩm dành cho thú cưng cũng như đặt các dịch vụ chăm sóc như cắt tỉa lông, tắm rửa và khách sạn thú cưng.

Trong workshop này, bạn sẽ học cách triển khai toàn bộ ứng dụng PetShop lên AWS theo kiến trúc đạt tiêu chuẩn triển khai thực tế (production) với tính sẵn sàng cao (High Availability). Phần giao diện React được phân phối toàn cầu thông qua **Amazon CloudFront** từ một **Amazon S3** Bucket riêng tư, trong khi API Laravel được triển khai trên các **Amazon EC2** nằm trong các Private Subnet, được quản lý bởi **Auto Scaling Group** và đặt phía sau **Application Load Balancer**. Dữ liệu của hệ thống được lưu trữ trên **Amazon RDS (MySQL)** và hình ảnh sản phẩm được lưu trên **Amazon S3**.

Bạn sẽ xây dựng hạ tầng theo từng lớp, trong đó mỗi lớp đảm nhiệm một vai trò riêng trong toàn bộ kiến trúc hệ thống:

+ **Lớp mạng (Network Layer)** – Xây dựng một VPC trải rộng trên hai Availability Zones, bao gồm Public Subnet, Private Subnet, NAT Gateway và S3 Gateway Endpoint, giúp các tài nguyên trong Private Subnet có thể truy cập Amazon S3 mà không cần đi qua Internet công cộng.

+ **Lớp ứng dụng (Application Layer)** – Triển khai Backend Laravel trên các EC2 trong Private Subnet phía sau Application Load Balancer, đồng thời triển khai Frontend React trên Amazon S3 kết hợp với Amazon CloudFront để phân phối website trên toàn cầu. CloudFront cũng đóng vai trò là điểm truy cập duy nhất, bảo mật cho cả giao diện người dùng và API.

#### Nội dung

1. [Giới thiệu Workshop](5.1-Workshop-overview)
2. [Điều kiện chuẩn bị](5.2-Prerequiste/)
3. [Tạo hạ tầng mạng VPC](5.3-VPC/)
4. [Tạo cơ sở dữ liệu RDS và S3 Bucket](5.4-Database-S3/)
5. [Triển khai Backend với EC2, ALB và Auto Scaling](5.5-Backend/)
6. [Triển khai Frontend với S3 và CloudFront](5.6-Frontend/)
7. [Kiểm thử ứng dụng](5.7-Testing/)
8. [Dọn dẹp tài nguyên](5.8-Cleanup/)
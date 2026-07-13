---
title : "Giới thiệu"
date : 2026-07-12
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu ứng dụng PetShop

+ **PetShop** là ứng dụng web full-stack cho cửa hàng thú cưng, kết hợp giữa **thương mại điện tử** (thức ăn, đồ chơi, phụ kiện cho thú cưng) và **hệ thống đặt lịch dịch vụ chăm sóc** (spa, tắm rửa, khách sạn chó mèo, cứu hộ thú cưng). Khách hàng có thể xem sản phẩm, thêm vào giỏ hàng, đặt khung giờ dịch vụ và thanh toán tất cả trong một lần checkout qua **Stripe** hoặc **VNPay**.
+ Ứng dụng gồm hai phần mã nguồn độc lập: giao diện người dùng dạng SPA viết bằng **React 18**, và REST API viết bằng **Laravel 10** đảm nhiệm nghiệp vụ, xác thực (Laravel Sanctum + đăng nhập Google), quản lý đơn hàng và gửi email thông báo. Việc tách riêng hai tầng giúp mỗi tầng được triển khai, mở rộng và bảo mật độc lập trên AWS.

#### Tổng quan về workshop

Trong workshop này, bạn sẽ triển khai ứng dụng PetShop lên AWS theo kiến trúc chuẩn production trải rộng trên **hai Availability Zone**:

+ **Amazon CloudFront** đóng vai trò cổng vào duy nhất và bảo mật của hệ thống. CloudFront phục vụ giao diện React từ một **Amazon S3** bucket riêng tư (thông qua Origin Access Control), đồng thời chuyển tiếp mọi request có đường dẫn `/api/*` về backend qua origin thứ hai — nhờ đó toàn bộ ứng dụng có HTTPS mà không cần mua tên miền riêng.
+ **API Laravel** chạy trên các **Amazon EC2** đặt trong **Private Subnet**, cách ly hoàn toàn khỏi Internet. **Application Load Balancer** ở Public Subnet phân phối lưu lượng đến các instance, và **Auto Scaling Group** tự động thay thế instance lỗi cũng như mở rộng khi CPU tăng cao.
+ **Amazon RDS (MySQL)** lưu trữ toàn bộ dữ liệu ứng dụng (sản phẩm, dịch vụ, đơn hàng, lịch hẹn, người dùng) trong các Private Subnet dành riêng cho database, chỉ có thể truy cập từ các instance ứng dụng thông qua quy tắc Security Group nối chuỗi.
+ **Amazon S3** lưu trữ toàn bộ hình ảnh sản phẩm và dịch vụ trong một bucket riêng. Các EC2 instance tải ảnh lên bucket này thông qua **S3 Gateway Endpoint**, nên lưu lượng ảnh không bao giờ đi qua NAT Gateway hay Internet công cộng.
+ **AWS Systems Manager Session Manager** được dùng để quản trị các EC2 instance trong Private Subnet trực tiếp từ trình duyệt, không cần SSH key hay bastion host.

#### Các dịch vụ AWS sử dụng trong workshop

| Dịch vụ | Vai trò trong kiến trúc |
|---|---|
| Amazon VPC | Nền tảng mạng: 6 subnet trên 2 AZ, Internet Gateway, NAT Gateway, S3 Gateway Endpoint |
| Amazon EC2 + Auto Scaling | Chạy API Laravel (Amazon Linux 2023, nginx + PHP 8.2) |
| Application Load Balancer | Phân phối lưu lượng API, health check qua `/api/v1/health` |
| Amazon RDS (MySQL 8.0) | Cơ sở dữ liệu ứng dụng |
| Amazon S3 | 3 bucket: hosting frontend, ảnh sản phẩm, artifacts triển khai |
| Amazon CloudFront | CDN toàn cầu + cổng vào HTTPS duy nhất cho cả frontend lẫn API |
| AWS IAM | IAM Role cho EC2 truy cập S3 và Session Manager |
| AWS Systems Manager | Truy cập shell các instance private qua trình duyệt |

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram01.png)

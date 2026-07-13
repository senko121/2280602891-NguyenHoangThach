---
title : "Triển khai Backend với EC2, ALB và Auto Scaling"
date : 2026-07-13
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tầng ứng dụng

Trong phần này, bạn sẽ đưa API Laravel vào hoạt động trên AWS:

+ **Target Group & Application Load Balancer** — cổng vào công khai của API, thực hiện health check qua endpoint `/api/v1/health` của ứng dụng.
+ **Launch Template** — bản thiết kế mô tả cách mọi instance API được dựng lên: Amazon Linux 2023, nginx + PHP 8.2, và một **user-data script** tự động tải mã nguồn cùng cấu hình từ S3 bucket riêng tư ngay khi khởi động.
+ **Auto Scaling Group** — duy trì số lượng instance mong muốn trên hai Private Subnet của tầng ứng dụng, tự động thay thế instance lỗi và mở rộng khi CPU tăng cao.
+ **Import database** — dùng **Session Manager** truy cập instance private ngay từ trình duyệt, import file SQL dump vào RDS và kiểm tra API từ đầu đến cuối.

Vì các instance được tạo **từ template + artifacts lưu trên S3**, cả đội máy chủ trở nên "dùng xong vứt được": bất kỳ instance nào cũng có thể bị terminate và một bản thay thế giống hệt sẽ tự khởi động mà không cần cấu hình tay — đây chính là tư tưởng cốt lõi của immutable infrastructure.

#### Nội dung

- [Tạo Target Group và ALB](5.5.1-alb-target-group/)
- [Tạo Launch Template](5.5.2-launch-template/)
- [Tạo Auto Scaling Group](5.5.3-auto-scaling/)
- [Import database và kiểm tra API](5.5.4-import-database/)

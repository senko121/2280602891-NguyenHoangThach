---
title : "Tạo Auto Scaling Group"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.5.3 </b> "
---

1. Ở thanh điều hướng EC2 console, chọn **Auto Scaling Groups** → **Create Auto Scaling group**:
+ **Auto Scaling group name**: ```petshop-asg```
+ **Launch template**: chọn **petshop-lt** → **Next**

2. Ở mục **Network**:
+ **VPC**: **petshop-vpc**
+ **Availability Zones and subnets**: chọn hai subnet **private app** — ```petshop-private-app-1a``` (`10.0.11.0/24`) và ```petshop-private-app-1b``` (`10.0.12.0/24`) → **Next**

![asg network](/images/5-Workshop/5.5-Backend/asg01.png)

3. Ở mục **Load balancing**:
+ Chọn **Attach to an existing load balancer** → **Choose from your load balancer target groups** → chọn **petshop-api-tg**
+ Ở *Health checks*, tick **Turn on Elastic Load Balancing health checks** — để ASG thay thế những instance rớt health check tầng ứng dụng `/api/v1/health`, chứ không chỉ dựa vào status check cơ bản của EC2 → **Next**

![asg load balancing](/images/5-Workshop/5.5-Backend/asg02.png)

4. Ở mục **Group size and scaling**:
+ **Desired capacity**: ```1``` — **Min**: ```1``` — **Max**: ```2```
+ **Automatic scaling**: chọn **Target tracking scaling policy**, metric **Average CPU utilization**, target value ```70```

{{% notice note %}}
Kiến trúc tham chiếu chạy **tối thiểu 2 instance** (mỗi AZ một chiếc) để đạt tính sẵn sàng cao. Workshop này chạy 1 để nằm trong Free Tier — ASG vẫn sẽ tự động khởi động instance thứ hai khi CPU trung bình vượt 70%, và thay thế instance bất cứ khi nào nó unhealthy.
{{% /notice %}}

5. Các màn hình còn lại giữ mặc định → **Create Auto Scaling group**.

6. Chờ 3–5 phút. Kiểm tra kết quả triển khai:
+ **EC2 → Instances**: một instance mới xuất hiện, do ASG khởi tạo trong private subnet (không có public IP).
+ **EC2 → Target Groups → petshop-api-tg → Targets**: trạng thái instance chuyển sang **healthy** khi user-data script chạy xong và nginx bắt đầu trả lời `/api/v1/health`.

![healthy target](/images/5-Workshop/5.5-Backend/asg03.png)

{{% notice tip %}}
Nếu target giữ trạng thái **unhealthy** quá ~5 phút, kết nối vào instance bằng Session Manager (mục 5.5.4) và xem log khởi động: ```cat /var/log/user-data.log``` — những dòng cuối cho biết chính xác bước nào thất bại (thường gặp nhất là sai tên bucket trong user-data script).
{{% /notice %}}

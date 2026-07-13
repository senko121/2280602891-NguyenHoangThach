---
title : "Tạo Target Group và ALB"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

#### Tạo Target Group

1. Mở [EC2 console](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Home:), ở thanh điều hướng chọn **Target Groups**, sau đó bấm **Create target group**:
+ **Choose a target type**: **Instances**
+ **Target group name**: ```petshop-api-tg```
+ **Protocol : Port**: **HTTP : 80**
+ **VPC**: **petshop-vpc**
+ Ở mục **Health checks**, đặt **Health check path** là: ```/api/v1/health```

{{% notice note %}}
`/api/v1/health` là route được viết riêng trong ứng dụng Laravel, trả về `{"status":"ok"}` mà không đụng tới database và không cần xác thực. ALB gọi nó mỗi 30 giây trên từng instance — instance nào ngừng phản hồi sẽ bị đánh dấu *unhealthy* và được Auto Scaling Group tự động thay thế.
{{% /notice %}}

![target group](/images/5-Workshop/5.5-Backend/alb01.png)

2. Bấm **Next**. Ở màn hình *Register targets*, **không** đăng ký instance nào — Auto Scaling Group sẽ tự động đăng ký ở mục 5.5.3. Bấm **Create target group**.

#### Tạo Application Load Balancer

3. Ở thanh điều hướng chọn **Load Balancers** → **Create load balancer** → dưới mục *Application Load Balancer* bấm **Create**:
+ **Load balancer name**: ```petshop-alb```
+ **Scheme**: **Internet-facing**
+ **IP address type**: **IPv4**

4. Ở mục **Network mapping**:
+ **VPC**: **petshop-vpc**
+ Tick cả hai Availability Zone và với mỗi AZ, chọn **public** subnet (`10.0.1.0/24` và `10.0.2.0/24`) — ⚠️ không chọn nhầm private subnet.

![network mapping](/images/5-Workshop/5.5-Backend/alb02.png)

5. Ở **Security groups**: bỏ `default`, chọn **petshop-alb-sg**.
6. Ở **Listeners and routing**: giữ **HTTP : 80** và đặt **Default action** forward về **petshop-api-tg**.
7. Bấm **Create load balancer** và chờ đến khi **State = Active**.

8. Mở trang chi tiết ALB và copy **DNS name** (dạng `petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com`).

![alb dns](/images/5-Workshop/5.5-Backend/alb03.png)

9. Cập nhật file `env.aws` trên máy cá nhân — điền DNS này vào `APP_URL`:

```
APP_URL=http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com
```

Sau đó upload lại `env.aws` lên bucket **petshop-deploy** (S3 console → bucket → **Upload**, ghi đè file cũ). Các instance đọc file này từ S3 lúc khởi động, nên bản trên bucket phải luôn là phiên bản mới nhất.

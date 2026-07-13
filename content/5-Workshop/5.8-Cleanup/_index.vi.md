---
title : "Dọn dẹp tài nguyên"
date : 2026-07-13
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

Chúc mừng bạn đã hoàn thành workshop! 🎉

Bạn đã triển khai một ứng dụng thương mại điện tử full-stack lên AWS với kiến trúc chuẩn production:
+ Một **VPC** trải rộng trên hai Availability Zone với bố cục subnet ba tầng, NAT Gateway và **S3 Gateway Endpoint** miễn phí.
+ Một đội **EC2 "dùng xong vứt được"** do Auto Scaling Group quản lý phía sau Application Load Balancer, tự dựng hoàn toàn từ artifacts trên S3 nhờ user-data script — không SSH, không cấu hình tay.
+ **Amazon RDS** trong các subnet database cách ly, chỉ truy cập được qua chuỗi Security Group.
+ Một **CloudFront** distribution duy nhất đóng vai trò cổng HTTPS cho cả frontend React (S3 riêng tư + OAC) lẫn API Laravel (behavior `/api/*`).

Để tránh phát sinh chi phí, hãy xóa tài nguyên **theo thứ tự ngược với lúc tạo** — phải gỡ những thứ phụ thuộc trước khi xóa thứ chúng phụ thuộc vào.

{{% notice warning %}}
💰 Hai tài nguyên âm thầm tốn tiền kể cả khi không dùng là **NAT Gateway (~$32/tháng)** và **Application Load Balancer (~$18/tháng)**. Nếu bạn chỉ muốn tạm dừng dự án thay vì xóa hết, hãy xóa ít nhất hai thứ này (và release Elastic IP).
{{% /notice %}}

#### 1. Xóa Auto Scaling Group và tài nguyên tính toán

+ **EC2 → Auto Scaling Groups** → chọn `petshop-asg` → **Delete** (gõ `delete` xác nhận). Các instance đang chạy sẽ tự bị terminate.
+ **EC2 → Load Balancers** → xóa `petshop-alb`.
+ **EC2 → Target Groups** → xóa `petshop-api-tg`.
+ **EC2 → Launch Templates** → xóa `petshop-lt`.

![delete asg](/images/5-Workshop/5.8-Cleanup/cleanup01.png)

#### 2. Xóa database

+ **RDS → Databases** → chọn `petshop-db` → **Actions → Delete**.
+ Bỏ tick *Create final snapshot*, tick ô xác nhận, gõ `delete me` để xác nhận.
+ Sau khi instance bị xóa, xóa tiếp `petshop-db-subnet-group` trong **Subnet groups**.

#### 3. Xóa NAT Gateway và release Elastic IP

+ **VPC → NAT gateways** → chọn NAT gateway → **Actions → Delete NAT gateway** (gõ `delete`).
+ ⚠️ Sau đó vào **VPC → Elastic IPs** → chọn địa chỉ vừa được giải phóng → **Actions → Release Elastic IP addresses** — Elastic IP không gắn vào đâu vẫn tốn ~$3.6/tháng.

![delete nat](/images/5-Workshop/5.8-Cleanup/cleanup02.png)

#### 4. Xóa VPC

+ **VPC → Your VPCs** → chọn `petshop-vpc` → **Actions → Delete VPC**. Thao tác này xóa luôn subnet, route table, Internet Gateway, S3 endpoint và security group trong một bước (gõ `delete` xác nhận).

#### 5. Làm rỗng và xóa các S3 bucket

Với từng bucket trong ba bucket (`petshop-frontend-...`, `petshop-images-...`, `petshop-deploy-...`):
+ Chọn bucket → **Empty** → gõ `permanently delete` → xác nhận.
+ Sau đó chọn lại bucket → **Delete** → gõ tên bucket → xác nhận.

![delete s3](/images/5-Workshop/5.8-Cleanup/cleanup03.png)

#### 6. Xóa CloudFront distribution

+ **CloudFront** → chọn distribution → **Disable** và chờ trạng thái được deploy xong (vài phút).
+ Khi đã disabled, chọn lại → **Delete**.

#### 7. Xóa tài nguyên IAM

+ **IAM → Roles** → xóa `petshop-ec2-role`.
+ **IAM → Policies** → xóa `petshop-s3-policy` (và các policy tùy chỉnh `petshop-*` khác nếu có).

#### 8. Kiểm tra lần cuối

Hôm sau mở **Billing → Bills** (hoặc Cost Explorer) và xác nhận không còn khoản phí nào liên quan PetShop tiếp tục phát sinh — chú ý đặc biệt các dòng EC2 (NAT Gateway), RDS và Elastic IP.

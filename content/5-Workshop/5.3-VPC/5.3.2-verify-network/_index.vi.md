---
title : "Kiểm tra và đặt tên mạng"
date : 2026-07-12
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

Wizard đặt tên bốn Private Subnet khá chung chung (`petshop-subnet-private1...4`). Đổi tên chúng theo tầng ngay bây giờ sẽ tránh chọn nhầm subnet khi tạo RDS subnet group (mục 5.4) và Auto Scaling Group (mục 5.5).

#### Đổi tên các Private Subnet

1. Ở thanh điều hướng của VPC console, chọn **Subnets** và lọc theo VPC ```petshop-vpc```.
2. Di chuột vào ô **Name** của từng Private Subnet, bấm biểu tượng bút chì và đổi tên theo CIDR:

| CIDR | Tên mới |
|---|---|
| `10.0.11.0/24` | `petshop-private-app-1a` |
| `10.0.12.0/24` | `petshop-private-app-1b` |
| `10.0.21.0/24` | `petshop-private-db-1a` |
| `10.0.22.0/24` | `petshop-private-db-1b` |

![subnets](/images/5-Workshop/5.3-VPC/verify01.png)

#### Kiểm tra route table

3. Ở thanh điều hướng, chọn **Route tables**. Chọn route table **public** (`petshop-rtb-public`) và mở tab **Routes** — phải có route `0.0.0.0/0` trỏ về **Internet Gateway** (`igw-xxxx`):

![public routes](/images/5-Workshop/5.3-VPC/verify02.png)

4. Chọn một trong các route table **private** và mở tab **Routes** — phải có **hai** route quan trọng:
+ `0.0.0.0/0` → **NAT Gateway** (`nat-xxxx`) — đường ra Internet cho các instance API.
+ Một managed prefix list (`pl-xxxx`, Amazon S3) → **VPC endpoint** (`vpce-xxxx`) — đây là route của S3 Gateway Endpoint, giúp lưu lượng S3 đi trong mạng nội bộ AWS.

![private routes](/images/5-Workshop/5.3-VPC/verify03.png)

#### Kiểm tra NAT Gateway

Wizard đã tự động cấu hình trọn vẹn NAT Gateway: cấp phát một **Elastic IP**, đặt gateway vào **public subnet**, và trỏ các route table private về nó (chính là route `nat-xxxx` thấy ở bước trước). Trong kiến trúc này, NAT Gateway là thứ cho phép các instance API trong private subnet thực hiện kết nối **chiều ra** — cài đặt phần mềm lúc khởi động, trừ tiền thẻ qua **Stripe API**, gửi email qua **Gmail SMTP**, và xác minh token **đăng nhập Google** — trong khi vẫn hoàn toàn không thể bị truy cập **chiều vào** từ Internet.

Ở thanh điều hướng, chọn **NAT gateways** và xác nhận: trạng thái **Available**, đã gắn Elastic IP, và subnet là một trong các **public** subnet.

![nat gateway](/images/5-Workshop/5.3-VPC/nat01.png)

#### Xem lại toàn bộ mạng

5. Ở thanh điều hướng, chọn **Your VPCs**, chọn ```petshop-vpc``` và mở tab **Resource map**. Xác nhận bức tranh hoàn chỉnh: 6 subnet trên 2 AZ, các route table, Internet Gateway, NAT Gateway và S3 endpoint — khớp với sơ đồ kiến trúc ở mục 5.1.

![resource map](/images/5-Workshop/5.3-VPC/verify04.png)

{{% notice tip %}}
💰 **Mẹo tiết kiệm chi phí:** NAT Gateway tính phí theo giờ kể cả khi không dùng. Nếu tạm dừng workshop vài ngày, bạn có thể xóa NAT Gateway (và release Elastic IP của nó), sau đó tạo lại và cập nhật route `0.0.0.0/0` trong các route table private trỏ về NAT mới.
{{% /notice %}}

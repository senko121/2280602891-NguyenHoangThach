---
title : "Import database và kiểm tra API"
date : 2026-07-13
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

#### Kết nối instance private bằng Session Manager

1. Ở EC2 console, chọn **Instances**, chọn instance do ASG khởi tạo, bấm **Connect** → mở tab **Session Manager** → **Connect**. Một cửa sổ shell mở ngay trong trình duyệt — không SSH key, không bastion host, không mở cổng 22.

![session manager](/images/5-Workshop/5.5-Backend/ssm01.png)

#### Import file SQL dump vào RDS

2. Trong phiên làm việc, chạy lần lượt các lệnh sau (thay `<ten-bucket-deploy>` và `<endpoint-RDS>`; nhập mật khẩu master của RDS khi được hỏi):

```bash
sudo -s
aws s3 cp s3://<ten-bucket-deploy>/petshop_dump.sql /tmp/
mysql -h <endpoint-RDS> -u admin -p petshop < /tmp/petshop_dump.sql
```

3. Kiểm tra kết quả import — các bảng và dữ liệu mẫu phải xuất hiện:

```bash
mysql -h <endpoint-RDS> -u admin -p petshop \
    -e "SHOW TABLES; SELECT COUNT(*) AS products FROM products;"
```

![import](/images/5-Workshop/5.5-Backend/ssm02.png)

{{% notice warning %}}
⚠️ Tuyệt đối không chạy `php artisan config:cache` trên các máy chủ này. Ứng dụng đọc một số biến (Stripe key, thông tin VNPay) bằng `env()` tại thời điểm chạy — khi config cache tồn tại, Laravel ngừng đọc `.env` hoàn toàn và mọi lời gọi `env()` trả về `null`, âm thầm làm hỏng cổng thanh toán. Nếu lỡ chạy rồi, sửa bằng `php artisan config:clear`.
{{% /notice %}}

#### Kiểm tra API qua ALB

4. Từ trình duyệt trên máy của bạn, mở hai URL sau (thay bằng DNS của ALB):

+ ```http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com/api/v1/health``` → phải trả về `{"status":"ok"}`
+ ```http://petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com/api/v1/viewHomePage``` → phải trả về JSON đầy đủ sản phẩm và dịch vụ đã import từ dump

![api test](/images/5-Workshop/5.5-Backend/api01.png)

Backend giờ đã vận hành hoàn chỉnh trên AWS: **Internet → ALB (public subnet) → EC2 (private subnet) → RDS (database subnet) + S3 (ảnh)**. Phần tiếp theo sẽ đưa giao diện React lên phía trước hệ thống này.

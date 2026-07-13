---
title : "Kiểm thử ứng dụng"
date : 2026-07-13
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Toàn bộ hệ thống đã chạy — giờ là lúc kiểm chứng các luồng nghiệp vụ quan trọng từ đầu đến cuối. Mọi bài test bên dưới thực hiện trên trình duyệt tại ```https://d2x5ledabb9h2k.cloudfront.net```.

#### 1. Trang chủ và hình ảnh

Mở website: trang chủ phải tải qua **HTTPS**, và ảnh sản phẩm phải hiển thị — chúng được phục vụ từ **bucket ảnh S3** (chuột phải vào một ảnh → *Mở ảnh trong tab mới* để xác nhận URL `s3.ap-southeast-1.amazonaws.com`).

#### 2. Xác thực

+ Đăng ký tài khoản mới, sau đó đăng nhập bằng tài khoản đó.
+ Đăng nhập bằng **Google** — bước này xác nhận cấu hình *Authorized JavaScript origins* ở mục 5.6.3.

![login](/images/5-Workshop/5.7-Testing/test01.png)

#### 3. Đặt hàng qua Stripe

Thêm sản phẩm vào giỏ và thanh toán bằng **thẻ test** của Stripe:

```
Số thẻ    : 4242 4242 4242 4242
Hết hạn   : bất kỳ ngày nào trong tương lai
CVC       : 3 số bất kỳ
```

Đơn hàng phải hoàn tất và chuyển đến trang cảm ơn. Điều này chứng minh chuỗi hoàn chỉnh: **CloudFront → ALB → EC2 → Stripe API (qua NAT Gateway) → RDS**.

![stripe order](/images/5-Workshop/5.7-Testing/test02.png)

#### 4. Theo dõi đơn hàng

Mở **Đơn hàng của tôi**: đơn mới xuất hiện kèm thanh theo dõi trạng thái, dữ liệu được đọc ngược từ **RDS**.

![my orders](/images/5-Workshop/5.7-Testing/test03.png)

#### 5. F5 trên SPA

Đứng tại một trang con bất kỳ (ví dụ trang chi tiết sản phẩm), nhấn **F5**. Trang phải tải lại bình thường thay vì hiện lỗi S3 — bước này kiểm chứng custom error response ở mục 5.6.1.

#### 6. Admin upload ảnh

Đăng nhập với tài khoản quản trị và tạo một sản phẩm mới kèm ảnh. Sau khi lưu:
+ Ảnh sản phẩm hiển thị trên website.
+ File ảnh xuất hiện trong bucket **petshop-images** (thư mục `product/`) — được EC2 instance upload thông qua **S3 Gateway Endpoint** bằng **IAM Role** của nó.

![admin upload](/images/5-Workshop/5.7-Testing/test04.png)

{{% notice tip %}}
Nếu bước nào thất bại, kiểm tra theo thứ tự: **F12 → Console/Network** trên trình duyệt xem request nào lỗi → cấu hình behavior CloudFront (mục 5.6.2) → `sudo tail /var/www/petshop/storage/logs/laravel.log` trên instance qua Session Manager.
{{% /notice %}}

Cả sáu bài kiểm tra đều đạt nghĩa là ứng dụng PetShop đã vận hành hoàn chỉnh trên AWS. 🎉

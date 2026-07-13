---
title : "Thêm origin và behavior cho API"
date : 2026-07-13
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

Distribution hiện mới chỉ phục vụ file tĩnh từ S3. Giờ dạy nó chuyển tiếp mọi request có đường dẫn bắt đầu bằng `/api/` về Application Load Balancer.

#### Thêm origin ALB

1. Mở distribution → tab **Origins** → **Create origin**:
+ **Origin domain**: dán **DNS name của ALB** (`petshop-alb-xxxx.ap-southeast-1.elb.amazonaws.com`)
+ **Protocol**: **HTTP only** — ALB không có chứng chỉ HTTPS; kết nối giữa người dùng và CloudFront vẫn là HTTPS
+ Bấm **Create origin**.

![alb origin](/images/5-Workshop/5.6-Frontend/cf05.png)

#### Tạo behavior /api/*

2. Mở tab **Behaviors** → **Create behavior**:
+ **Path pattern**: ```/api/*```
+ **Origin**: chọn origin ALB vừa tạo ở bước 1
+ **Viewer protocol policy**: **Redirect HTTP to HTTPS**
+ **Allowed HTTP methods**: **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE** — API cần đủ cả (đăng nhập là POST, cập nhật trạng thái đơn là PUT, xóa ảnh là DELETE...)
+ **Cache policy**: **CachingDisabled** — phản hồi API là dữ liệu động, tuyệt đối không được cache
+ **Origin request policy**: **AllViewer** — chuyển tiếp toàn bộ header (bao gồm token `Authorization`), cookie và query string về backend
+ Bấm **Create behavior**.

![api behavior](/images/5-Workshop/5.6-Frontend/cf06.png)

{{% notice warning %}}
⚠️ Ba thiết lập trên màn hình này nếu bỏ sót sẽ âm thầm phá hỏng ứng dụng: **đủ 7 HTTP method** (thiếu là đăng nhập/thanh toán thất bại), **CachingDisabled** (thiếu là giỏ hàng của người này có thể bị cache trả cho người khác), và **AllViewer** (thiếu là header `Authorization` bị lược bỏ, mọi request cần đăng nhập đều trả về 401).
{{% /notice %}}

3. Kiểm tra tab **Behaviors** giờ có hai dòng, trong đó ```/api/*``` xếp **trên** ```Default (*)``` — CloudFront luôn so khớp đường dẫn cụ thể nhất trước.

![behaviors list](/images/5-Workshop/5.6-Frontend/cf07.png)

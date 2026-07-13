---
title : "Triển khai Frontend với S3 và CloudFront"
date : 2026-07-13
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tầng giao diện

Trong phần này, bạn sẽ triển khai giao diện React và kết nối nó với backend — bằng một quyết định kiến trúc thông minh: **một CloudFront distribution duy nhất phục vụ cả website lẫn API**.

+ **Behavior mặc định** phục vụ các file tĩnh React từ một **S3 bucket riêng tư** thông qua **Origin Access Control (OAC)** — bucket không bao giờ bị lộ ra Internet.
+ **Behavior thứ hai** khớp đường dẫn ```/api/*``` chuyển tiếp các request API về origin **Application Load Balancer**.

Thiết kế này mang lại **HTTPS trên domain miễn phí `*.cloudfront.net`** cho toàn bộ ứng dụng mà không cần mua tên miền riêng hay chứng chỉ ACM, đồng thời né được **vấn đề mixed-content kinh điển** (trang HTTPS không được phép gọi API HTTP — với một distribution duy nhất, cả trang lẫn API dùng chung một nguồn HTTPS).

#### Nội dung

- [Tạo bucket frontend và CloudFront distribution](5.6.1-cloudfront-s3/)
- [Thêm origin và behavior cho API](5.6.2-api-behavior/)
- [Build, upload và kết nối toàn hệ thống](5.6.3-build-deploy/)

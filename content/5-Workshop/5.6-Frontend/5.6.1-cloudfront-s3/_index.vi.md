---
title : "Tạo bucket frontend và CloudFront distribution"
date : 2026-07-13
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

#### Tạo bucket frontend riêng tư

1. Mở [S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1) → **Create bucket**:
+ **Bucket name**: ```petshop-frontend-<ten-rieng-cua-ban>```
+ **Block Public Access settings**: **giữ nguyên tick "Block all public access"** ✅ — khác với bucket ảnh, không ai truy cập thẳng bucket này; chỉ CloudFront đọc nó qua OAC.
+ Bấm **Create bucket**.

#### Tạo CloudFront distribution

2. Mở [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home) → **Create distribution**:
+ **Origin domain**: chọn bucket ```petshop-frontend-...``` trong dropdown
+ **Origin access**: chọn **Origin access control settings (recommended)** → bấm **Create new OAC** → giữ mặc định → **Create**

![origin](/images/5-Workshop/5.6-Frontend/cf01.png)

3. Ở **Default cache behavior**:
+ **Viewer protocol policy**: **Redirect HTTP to HTTPS**
4. **Web Application Firewall (WAF)**: chọn **Do not enable security protections** (WAF tốn ~$8+/tháng; có thể bật lại sau chỉ với một cú click).
5. Ở mục **Settings**, đặt **Default root object**: ```index.html``` → bấm **Create distribution**.

6. Ngay sau khi tạo, một banner xanh xuất hiện: *"Complete distribution configuration by allowing CloudFront to access the S3 bucket"* — bấm **Copy policy**, sau đó mở S3 bucket → **Permissions** → **Bucket policy** → **Edit** → dán → **Save changes**. Policy này chỉ cho phép **đúng distribution CloudFront này** đọc bucket.

![bucket policy](/images/5-Workshop/5.6-Frontend/cf02.png)

#### Cấu hình error response cho React SPA

7. Ứng dụng React là Single-Page Application: các route như `/collections/dog-food` chỉ tồn tại trong trình duyệt, không phải file thật trên S3. Khi khách F5 tại các trang đó, S3 trả về lỗi — cần dạy CloudFront trả về `index.html` thay thế để React Router tiếp quản.

Mở distribution → tab **Error pages** → **Create custom error response**, tạo **hai** quy tắc:

| Thiết lập | Quy tắc 1 | Quy tắc 2 |
|---|---|---|
| HTTP error code | **403: Forbidden** | **404: Not Found** |
| Customize error response | **Yes** | **Yes** |
| Response page path | ```/index.html``` | ```/index.html``` |
| HTTP response code | **200: OK** | **200: OK** |

![error pages](/images/5-Workshop/5.6-Frontend/cf03.png)

8. Ở tab **General**, copy **Distribution domain name** (dạng `d1a2b3c4.cloudfront.net`) — đây chính là địa chỉ công khai của website PetShop.

![domain](/images/5-Workshop/5.6-Frontend/cf04.png)

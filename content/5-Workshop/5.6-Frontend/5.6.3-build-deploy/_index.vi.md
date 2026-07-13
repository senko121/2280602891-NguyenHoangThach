---
title : "Build, upload và kết nối toàn hệ thống"
date : 2026-07-13
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

#### Build ứng dụng React cho production

1. Trên máy cá nhân, tạo file ```react-project/.env.production``` — Create React App tự động đọc nó khi chạy `npm run build` và "nướng" các giá trị vào bundle:

```
REACT_APP_BASE_URL=https://<domain-cloudfront-cua-ban>
REACT_APP_API_URL=https://<domain-cloudfront-cua-ban>/api/v1
REACT_APP_IMAGE_URL=https://petshop-images-<ten-rieng-cua-ban>.s3.ap-southeast-1.amazonaws.com
REACT_APP_GOOGLE_CLIENT_ID=<google-oauth-client-id>
```

{{% notice note %}}
`REACT_APP_API_URL` trỏ về **chính domain CloudFront** kèm tiền tố `/api/v1` — nhờ behavior đã tạo ở 5.6.2, frontend và API dùng chung một nguồn HTTPS. Còn ảnh được tải thẳng từ bucket ảnh công khai.
{{% /notice %}}

2. Build:

```powershell
cd react-project
npm run build
```

Bundle production được sinh ra trong thư mục ```build/```.

![build](/images/5-Workshop/5.6-Frontend/deploy01.png)

#### Upload lên bucket frontend

3. Mở bucket ```petshop-frontend-...``` → **Upload** → kéo thả **phần ruột bên trong thư mục `build/`** (các file `index.html`, `favicon.ico`... và các thư mục `static/`, `assets/`) — ⚠️ không kéo nguyên thư mục `build`: file `index.html` phải nằm ngay gốc bucket.

![upload](/images/5-Workshop/5.6-Frontend/deploy02.png)

4. Nếu là lần deploy lại, hãy xóa cache CloudFront để khách nhận bản build mới ngay lập tức: distribution → tab **Invalidations** → **Create invalidation** → object paths: ```/*```.

#### Trỏ backend về frontend mới

5. Cập nhật ```env.aws``` trên máy cá nhân với domain CloudFront, rồi upload lên bucket **petshop-deploy** (ghi đè):

```
FRONTEND_URL=https://<domain-cloudfront-cua-ban>
CORS_ALLOWED_ORIGINS=https://<domain-cloudfront-cua-ban>
```

6. Làm mới instance đang chạy qua **Session Manager** (các instance sinh ra sau này tự lấy file mới lúc khởi động):

```bash
sudo aws s3 cp s3://<ten-bucket-deploy>/env.aws /var/www/petshop/.env
sudo chown nginx:nginx /var/www/petshop/.env
cd /var/www/petshop && sudo php artisan config:clear
```

#### Đăng ký domain với Google OAuth

7. Mở [Google Cloud Console](https://console.cloud.google.com/) → **APIs & Services → Credentials** → mở OAuth 2.0 Client ID → ở mục **Authorized JavaScript origins** bấm **Add URI** và nhập ```https://<domain-cloudfront-cua-ban>``` (không dấu `/` cuối, không kèm path) → **Save**. Thiếu bước này, đăng nhập Google sẽ báo *Error 400: origin_mismatch*. Thay đổi có hiệu lực sau 5 phút đến vài giờ.

![google oauth](/images/5-Workshop/5.6-Frontend/deploy03.png)

8. Mở ```https://<domain-cloudfront-cua-ban>``` — trang chủ PetShop hiển thị qua HTTPS với ảnh sản phẩm được phục vụ từ S3. 🎉

![website](/images/5-Workshop/5.6-Frontend/deploy04.png)

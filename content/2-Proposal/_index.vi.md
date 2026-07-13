---
title: "Bản đề xuất"
date: "2026-05-12"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Trong phần này, nhóm sẽ trình bày giải pháp đề xuất để triển khai nền tảng **PetCare Shop** trên Amazon Web Services (AWS).

# PetCare Shop Platform
## Kiến trúc AWS có khả năng mở rộng cho hệ thống quản lý cửa hàng và đặt lịch chăm sóc thú cưng

### 1. Tóm tắt điều hành

PetCare Shop là một ứng dụng web được xây dựng nhằm hỗ trợ quản lý cửa hàng thú cưng kết hợp với chức năng đặt lịch chăm sóc thú cưng. Hệ thống cho phép khách hàng xem sản phẩm, đặt hàng và đặt lịch chăm sóc thú cưng trực tuyến, đồng thời hỗ trợ quản trị viên quản lý sản phẩm, đơn hàng, lịch hẹn và thông tin khách hàng thông qua một giao diện quản trị tập trung.

Hệ thống được triển khai trên nền tảng AWS với kiến trúc có khả năng mở rộng, tính sẵn sàng cao và bảo mật tốt nhằm nâng cao hiệu năng, giảm chi phí vận hành và đáp ứng nhu cầu phát triển trong tương lai.

---

## 2. Tuyên bố vấn đề

### Vấn đề hiện tại

Nhiều cửa hàng thú cưng hiện nay vẫn quản lý việc bán hàng và đặt lịch bằng phương pháp thủ công hoặc sử dụng các hệ thống riêng lẻ, gây khó khăn trong việc quản lý dữ liệu tập trung và mở rộng khi số lượng người dùng tăng lên. Ngoài ra, việc triển khai toàn bộ ứng dụng trên một máy chủ duy nhất dễ dẫn đến điểm lỗi đơn (Single Point of Failure), làm giảm tính ổn định và khả năng sẵn sàng của hệ thống.

### Giải pháp

PetCare Shop được triển khai theo mô hình kiến trúc nhiều tầng trên AWS. Người dùng truy cập hệ thống thông qua Amazon Route 53, Amazon CloudFront và AWS WAF trước khi yêu cầu được chuyển đến Application Load Balancer. Load Balancer sẽ phân phối lưu lượng đến các máy chủ Amazon EC2 trong Auto Scaling Group để xử lý nghiệp vụ. Dữ liệu được lưu trữ trên Amazon RDS, hình ảnh sản phẩm và các tệp tĩnh được lưu trên Amazon S3, trong khi Amazon CloudWatch được sử dụng để giám sát hoạt động của toàn bộ hệ thống.

### Lợi ích và hoàn vốn đầu tư (ROI)

Kiến trúc AWS giúp tăng khả năng sẵn sàng, mở rộng linh hoạt và nâng cao bảo mật cho hệ thống. Auto Scaling tự động điều chỉnh số lượng máy chủ theo lưu lượng truy cập, CloudFront cải thiện tốc độ tải trang thông qua cơ chế CDN, còn Amazon S3 giúp giảm tải lưu trữ cho máy chủ ứng dụng. Mặc dù chi phí triển khai cao hơn so với các dịch vụ lưu trữ truyền thống, nhưng hệ thống mang lại nền tảng ổn định, dễ bảo trì và sẵn sàng mở rộng khi quy mô kinh doanh phát triển.

---

## 3. Kiến trúc giải pháp

Hệ thống được xây dựng theo mô hình kiến trúc Web Application trên AWS. Mọi yêu cầu từ người dùng sẽ được phân giải tên miền thông qua Amazon Route 53 và tăng tốc truy cập bằng Amazon CloudFront. AWS WAF sẽ kiểm tra và lọc các yêu cầu độc hại trước khi chuyển đến Application Load Balancer.

Application Load Balancer phân phối lưu lượng đến nhiều máy chủ Amazon EC2 được triển khai trong Private Subnet và quản lý bởi Auto Scaling Group. Dữ liệu của hệ thống được lưu trên Amazon RDS, trong khi hình ảnh sản phẩm và các tệp tải lên được lưu trên Amazon S3. Các EC2 truy cập Internet thông qua NAT Gateway khi cần thiết và Amazon CloudWatch được sử dụng để theo dõi hiệu năng, nhật ký và trạng thái hoạt động của toàn bộ hệ thống.

![PetCare Shop Architecture](/images/2-Proposal/petcare_architecture.png)

### Dịch vụ AWS sử dụng

- **Amazon Route 53:** Quản lý tên miền và phân giải DNS.
- **Amazon CloudFront:** Phân phối nội dung và tăng tốc truy cập thông qua CDN.
- **AWS WAF:** Bảo vệ ứng dụng khỏi các cuộc tấn công phổ biến trên web.
- **Application Load Balancer:** Phân phối lưu lượng đến các máy chủ EC2.
- **Amazon EC2:** Chạy ứng dụng backend.
- **Amazon EC2 Auto Scaling:** Tự động mở rộng hoặc thu hẹp số lượng EC2 theo tải hệ thống.
- **Amazon RDS:** Lưu trữ cơ sở dữ liệu của hệ thống.
- **Amazon S3:** Lưu trữ mã nguồn frontend, hình ảnh sản phẩm và các tệp tĩnh.
- **NAT Gateway:** Cho phép EC2 trong Private Subnet truy cập Internet.
- **Amazon CloudWatch:** Giám sát hệ thống và thu thập nhật ký hoạt động.

### Thiết kế thành phần

- **Frontend:** Lưu trữ trên Amazon S3 và phân phối qua CloudFront.
- **Backend:** Ứng dụng Spring Boot chạy trên Amazon EC2.
- **Cơ sở dữ liệu:** Amazon RDS MySQL.
- **Lưu trữ hình ảnh:** Amazon S3.
- **Cân bằng tải:** Application Load Balancer.
- **Mở rộng hệ thống:** Amazon EC2 Auto Scaling Group.
- **Giám sát:** Amazon CloudWatch.
- **Bảo mật:** AWS WAF kết hợp với Private Subnet và NAT Gateway.

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai

Dự án được triển khai theo bốn giai đoạn:

1. Thiết kế kiến trúc AWS và tính toán chi phí triển khai.
2. Xây dựng hạ tầng mạng, bảo mật và các tài nguyên trên AWS.
3. Phát triển và triển khai frontend, backend cùng cơ sở dữ liệu.
4. Kiểm thử, tối ưu hiệu năng và đưa hệ thống vào vận hành.

### Yêu cầu kỹ thuật

- **Frontend:** ReactJS.
- **Backend:** Spring Boot REST API.
- **Cơ sở dữ liệu:** MySQL trên Amazon RDS.
- **Hạ tầng AWS:** Amazon EC2, Application Load Balancer, Auto Scaling Group, Amazon Route 53, Amazon CloudFront, AWS WAF, Amazon S3, NAT Gateway và Amazon CloudWatch.

---

## 5. Lộ trình & Mốc triển khai

### Kế hoạch thực hiện

- **Tháng 1:** Phân tích yêu cầu và thiết kế kiến trúc AWS.
- **Tháng 2:** Triển khai hạ tầng và phát triển hệ thống.
- **Tháng 3:** Kiểm thử, tối ưu và triển khai chính thức.

---

## 6. Ước tính ngân sách

Chi phí hạ tầng được ước tính dựa trên **AWS Pricing Calculator** tại khu vực **Singapore (ap-southeast-1)**.

### Chi phí hạ tầng

- Amazon EC2 (2 × t3.micro): **khoảng 30 USD/tháng**.
- Amazon RDS MySQL (db.t3.micro): **khoảng 18 USD/tháng**.
- Application Load Balancer: **khoảng 18 USD/tháng**.
- Amazon CloudFront: **khoảng 5 USD/tháng**.
- Amazon S3 (Frontend): **khoảng 0,10 USD/tháng**.
- Amazon S3 (Hình ảnh): **khoảng 1 USD/tháng**.
- Amazon Route 53: **khoảng 0,50 USD/tháng**.
- AWS WAF: **khoảng 7 USD/tháng**.
- Amazon CloudWatch: **khoảng 3 USD/tháng**.
- Amazon EC2 Auto Scaling Group: **Không phát sinh chi phí**.
- NAT Gateway (02 Gateway và chi phí truyền dữ liệu ước tính): **khoảng 100 USD/tháng**.

**Tổng chi phí ước tính:** **khoảng 182,60 USD/tháng**, tương đương **2.191,20 USD/năm**.

> **Lưu ý:** Chi phí trên chỉ mang tính chất tham khảo, được ước tính theo AWS Pricing Calculator và có thể thay đổi tùy theo mức sử dụng thực tế cũng như chính sách giá của AWS.

---

## 7. Đánh giá rủi ro

### Ma trận rủi ro

- EC2 gặp sự cố: Mức ảnh hưởng trung bình, xác suất xảy ra thấp.
- Cơ sở dữ liệu gặp sự cố: Mức ảnh hưởng cao, xác suất xảy ra thấp.
- Lưu lượng truy cập tăng đột biến: Mức ảnh hưởng trung bình, xác suất xảy ra trung bình.
- Tấn công bảo mật: Mức ảnh hưởng cao, xác suất xảy ra trung bình.

### Giải pháp giảm thiểu

- Auto Scaling tự động thay thế các EC2 không còn hoạt động.
- Amazon RDS sử dụng cơ chế sao lưu tự động để bảo vệ dữ liệu.
- AWS WAF ngăn chặn các truy cập độc hại.
- Amazon CloudWatch gửi cảnh báo khi phát hiện sự cố hoặc hoạt động bất thường.

### Kế hoạch dự phòng

- Khôi phục cơ sở dữ liệu từ bản sao lưu của Amazon RDS.
- Tự động khởi tạo EC2 mới thông qua Auto Scaling Group.
- Mở rộng tài nguyên khi lưu lượng truy cập tăng cao.

---

## 8. Kết quả kỳ vọng

### Cải tiến kỹ thuật

Xây dựng thành công một hệ thống thương mại điện tử có khả năng mở rộng, tính sẵn sàng cao và bảo mật tốt, đáp ứng nhu cầu bán sản phẩm và đặt lịch chăm sóc thú cưng trực tuyến.

### Giá trị lâu dài

Kiến trúc đề xuất tạo nền tảng cho việc phát triển các tính năng mới như thanh toán trực tuyến, triển khai CI/CD, chuyển đổi sang Amazon ECS hoặc Microservices và tích hợp các tính năng AI trong tương lai.
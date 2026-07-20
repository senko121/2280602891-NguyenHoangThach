---
title: "Blog 2"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# TỐI ƯU CHI PHÍ NAT GATEWAY TRÊN AWS

Chào mọi người 👋

Nếu bạn đã đang làm việc với AWS và triển khai các hệ thống trên VPC, chắc chắn bạn đã nghe đến **NAT Gateway**.

### NAT Gateway là gì?

Khi chúng ta đặt EC2 hay ứng dụng trong **Private Subnet**, chúng không có Public IP nên không thể truy cập Internet trực tiếp. Nhưng đôi khi chúng vẫn cần kết nối ra ngoài để cập nhật hệ điều hành, tải thư viện từ npm hoặc Maven, gọi API của bên thứ ba, pull Docker image, hoặc kết nối dịch vụ bên ngoài khác.

Đây chính là lúc **NAT Gateway** phát huy tác dụng. NAT Gateway là dịch vụ được AWS quản lý hoàn toàn, giúp các tài nguyên Private có thể chủ động truy cập Internet mà vẫn an toàn.

### Vấn đề: NAT Gateway có thể đốt sạch ngân sách

AWS tính phí NAT Gateway dựa trên ba yếu tố chính:

* **Thời gian hoạt động** — NAT Gateway chạy 24/7 dù có traffic hay không.
* **Dung lượng dữ liệu xử lý** — tính theo per GB.
* **Phí truyền dữ liệu** — phát sinh nếu dữ liệu đi qua các AZ khác nhau.

**Ví dụ minh họa:**

Giả sử hệ thống của bạn có **3 Availability Zone**. AZ1 có 1 NAT Gateway cộng 1.000 GB traffic mỗi tháng. AZ2 và AZ3 cũng vậy.

Chi phí mỗi AZ được tính như sau:

* Phí theo giờ: 0,045 × 730 giờ = **32,85 USD**.
* Phí xử lý dữ liệu: 0,045 × 1.000 GB = **45 USD**.
* **Tổng cộng mỗi AZ: 77,85 USD/tháng.**

Chi phí ba AZ cộng lại: 77,85 × 3 = **233,55 USD/tháng**.

Vậy có cách nào để giảm con số này không?

### Giải pháp thứ nhất: Xác định traffic

Trước khi tối ưu, cần biết dữ liệu thực sự đi đến đâu. Bạn cần trả lời các câu hỏi như:

* EC2 nào tạo traffic nhiều nhất?
* Traffic có đi ra Internet hay chỉ kết nối dịch vụ AWS?
* Bao nhiêu traffic là đi đến S3?
* Có NAT Gateway nào không được sử dụng không?

Các công cụ để phân tích gồm:

* **Amazon CloudWatch** để theo dõi các metric như `BytesInFromSource`, `BytesOutToDestination`, `ConnectionAttemptCount`.
* **VPC Flow Logs** để ghi chi tiết traffic vào hoặc ra khỏi VPC.
* **Amazon S3** để lưu trữ VPC Flow Logs.
* **Amazon Athena** để truy vấn log bằng SQL và tìm ra những "top talkers".

**Ví dụ phân tích:** sau khi phân tích VPC Flow Logs, bạn phát hiện rằng **50% traffic đi đến Amazon S3**, 30% gọi API bên ngoài, 15% tải package và cập nhật, và 5% đi tới dịch vụ khác.

### Giải pháp thứ hai: Sử dụng S3 Gateway Endpoint

Trong ví dụ trên, 50% traffic đi đến S3. Chúng ta không nhất thiết phải cho dữ liệu này đi qua NAT Gateway.

**Cách hoạt động khi không có S3 Endpoint:** EC2 Private đi tới NAT Gateway, sau đó qua Internet, rồi mới tới S3.

**Sau khi có S3 Endpoint:** EC2 Private đi thẳng tới S3 Gateway Endpoint, rồi tới S3 nội bộ AWS — không cần đi qua NAT Gateway hay Internet công cộng.

**Chi phí sau khi thêm S3 Endpoint** được tính như sau: giả sử 50% traffic không cần đi qua NAT, mỗi AZ còn 500 GB.

* Chi phí NAT theo giờ: **32,85 USD**.
* Phí xử lý 500 GB: 0,045 × 500 = **22,50 USD**.
* **Tổng mỗi AZ: 55,35 USD/tháng.**

Tổng ba AZ: 55,35 × 3 = **166,05 USD/tháng**.

So với ba NAT Gateway không có endpoint (233,55 USD/tháng), ta tiết kiệm được **67,50 USD/tháng**.

![AWS NAT Gateway Cost Optimization](/images/3-Blog/aws_NAT.png)

**Tham khảo:**

https://vegacloud.medium.com/aws-nat-gateway-optimization-bd5f7d2da8a8

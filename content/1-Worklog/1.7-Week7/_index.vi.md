---

title: "Worklog Tuần 7"
date: 2026-06-07
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
----------------------

### Mục tiêu tuần 7:

* Hiểu và áp dụng quy trình CI/CD để tự động hóa việc triển khai ứng dụng trên AWS.
* Tìm hiểu các giải pháp giám sát hệ thống và quản lý log tập trung cho môi trường container.
* Làm quen với các dịch vụ bảo mật và đánh giá tuân thủ trên AWS.
* Tìm hiểu và triển khai các giải pháp kết nối giữa nhiều VPC trong hệ thống mạng AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                          |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------- |
| 2   | - Tìm hiểu khái niệm CI/CD <br> - Thực hành xây dựng pipeline triển khai tự động bằng GitLab CI/CD, GitHub Actions và AWS CodeBuild                                                 | 06/01/2026   | 06/01/2026      | https://cloudjourney.awsstudygroup.com/ |
| 3   | - Tìm hiểu CloudWatch Container Insights <br> - Thực hành cấu hình FireLens để chuyển log ứng dụng đến Amazon S3                                                                    | 06/02/2026   | 06/02/2026      | https://cloudjourney.awsstudygroup.com/ |
| 4   | - Dọn dẹp các tài nguyên AWS (ECS, ECR, RDS, VPC, ...) theo nguyên tắc Outside-In nhằm tránh phát sinh chi phí không cần thiết                                                      | 06/03/2026   | 06/03/2026      | https://cloudjourney.awsstudygroup.com/ |
| 5   | - Tìm hiểu AWS Security Hub <br> - Tìm hiểu AWS Config <br> - Đánh giá mức độ tuân thủ bảo mật dựa trên các khuyến nghị của AWS                                                     | 06/04/2026   | 06/05/2026      | https://cloudjourney.awsstudygroup.com/ |
| 6   | - Triển khai hạ tầng mạng bằng CloudFormation <br> - Cấu hình VPC Peering và Cross-Peer DNS <br> - Cấu hình Route Tables <br> - Triển khai AWS Transit Gateway để kết nối nhiều VPC | 06/06/2026   | 06/07/2026      | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 7:

* Triển khai thành công các pipeline CI/CD bằng:

  * GitLab CI/CD
  * GitHub Actions
  * AWS CodeBuild

* Hiểu được quy trình tự động hóa triển khai ứng dụng từ giai đoạn build, test đến deploy.

* Cấu hình CloudWatch Container Insights để theo dõi các chỉ số hiệu năng của container và dịch vụ đang chạy.

* Thiết lập FireLens để tự động thu thập và chuyển log ứng dụng đến Amazon S3 phục vụ việc lưu trữ tập trung.

* Thực hiện dọn dẹp các tài nguyên AWS theo đúng quy trình nhằm tối ưu chi phí sử dụng dịch vụ.

* Cấu hình AWS Security Hub và AWS Config để theo dõi tình trạng bảo mật và mức độ tuân thủ của hệ thống.

* Triển khai hạ tầng mạng bằng AWS CloudFormation.

* Thiết lập VPC Peering kết hợp Cross-Peer DNS và Route Tables để cho phép các EC2 Instance thuộc các VPC khác nhau giao tiếp với nhau.

* Triển khai AWS Transit Gateway để kết nối tập trung nhiều VPC trong cùng hệ thống.

* Hoàn thành thành công Lab17, Lab18, Lab19 và Lab20.

* Nâng cao kiến thức về tự động hóa hạ tầng, giám sát hệ thống, bảo mật đám mây và kiến trúc mạng đa VPC trên AWS.

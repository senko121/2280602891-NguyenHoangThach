---
title: "Các bài viết đã chia sẻ"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong quá trình thực tập, em đã chủ động nghiên cứu các công nghệ mới trên nền tảng AWS và chia sẻ lại dưới dạng các bài viết kỹ thuật trên cộng đồng **AWS Study Group**. Các bài blog tập trung vào những tính năng mới của AWS cũng như các giải pháp giúp xây dựng, vận hành và tối ưu hệ thống AI hiện đại trên nền tảng Amazon Bedrock.

Danh sách các bài blog đã thực hiện bao gồm:

### [Blog 1 - XÂY DỰNG TRÌNH CHỈNH SỬA HÌNH ẢNH KHÔNG MÁY CHỦ VỚI AMAZON BEDROCK AGENTCORE HARNESS](3.1-Blog1/)

Bài viết hướng dẫn cách xây dựng một trình chỉnh sửa hình ảnh không máy chủ, nơi người dùng tải ảnh lên, mô tả chỉnh sửa bằng ngôn ngữ tự nhiên và nhận kết quả trong vài giây. Nội dung tập trung vào **Amazon Bedrock AgentCore Harness** — thành phần đảm nhiệm toàn bộ việc điều phối AI Agent, quản lý bộ nhớ hội thoại, định tuyến công cụ qua Model Context Protocol (MCP) và cung cấp môi trường thực thi microVM cô lập.

### [Blog 2 - TỐI ƯU CHI PHÍ NAT GATEWAY TRÊN AWS](3.2-Blog2/)

Bài viết phân tích cách NAT Gateway tính phí trên AWS và vì sao chi phí có thể tăng rất nhanh khi hệ thống trải rộng trên nhiều Availability Zone. Nội dung hướng dẫn cách xác định traffic thực tế bằng VPC Flow Logs, Amazon Athena và CloudWatch, sau đó áp dụng **S3 Gateway Endpoint** để loại bỏ phần lớn traffic đi đến S3 ra khỏi NAT Gateway, giúp tiết kiệm đáng kể chi phí vận hành hằng tháng.

### [Blog 3 - WEB SEARCH TRONG AMAZON BEDROCK AGENTCORE](3.3-Blog3/)

Bài viết giới thiệu tính năng **Web Search** trong Amazon Bedrock AgentCore, cho phép AI Agent truy cập thông tin theo thời gian thực từ Internet mà không cần tích hợp các dịch vụ tìm kiếm của bên thứ ba. Nội dung tập trung vào kiến trúc hoạt động, Amazon Web Index, Semantic Snippet Extraction, Knowledge Graph, khả năng tích hợp với các AI Framework và những lợi ích về bảo mật cũng như khả năng mở rộng khi xây dựng các ứng dụng AI hiện đại.
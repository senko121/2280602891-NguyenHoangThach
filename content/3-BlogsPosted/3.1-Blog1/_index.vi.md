---
title: "Blog 1"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# XÂY DỰNG TRÌNH CHỈNH SỬA HÌNH ẢNH KHÔNG MÁY CHỦ VỚI AMAZON BEDROCK AGENTCORE HARNESS

Bài viết này với mục đích chia sẻ và hướng dẫn cách xây dựng một trình chỉnh sửa hình ảnh không máy chủ, nơi người dùng tải ảnh lên, mô tả chỉnh sửa bằng tiếng Anh thông thường và nhận kết quả trong vài giây.

Điểm đặc biệt của giải pháp là sử dụng **Amazon Bedrock AgentCore Harness** để đảm nhiệm toàn bộ quá trình điều phối AI Agent, bao gồm quản lý vòng lặp suy luận, lựa chọn công cụ, lưu trữ bộ nhớ hội thoại và cung cấp môi trường thực thi.

AgentCore Harness sẽ chạy agent trong một microVM có trạng thái và được cô lập, đồng thời tích hợp sẵn bộ nhớ, định tuyến công cụ và khả năng giám sát.

### Cách ứng dụng hoạt động

AI Agent trong ứng dụng sử dụng **Claude Sonnet 4.6** để phân tích yêu cầu chỉnh sửa thành nhiều bước nhỏ. Sau đó, agent lựa chọn và gọi công cụ tương ứng với từng mô hình Stability AI để thực hiện chỉnh sửa ảnh.

Khi ảnh đã được xử lý, một chương trình Python chạy trực tiếp trong microVM để thêm watermark. Thao tác này không cần mô hình AI suy luận nên không tiêu tốn token.

### Những khả năng nổi bật

**1️⃣ Tạo AI Agent bằng cấu hình**

Agent được định nghĩa hoàn toàn thông qua các tham số API. Không cần tự viết mã điều phối, sử dụng framework riêng hoặc xây dựng container.

**2️⃣ Chuyển đổi mô hình theo từng yêu cầu**

Frontend sử dụng **Claude Haiku 4.5** cho các cuộc hội thoại đơn giản và **Claude Sonnet 4.6** cho những yêu cầu chỉnh sửa ảnh phức tạp.

**3️⃣ Thay đổi persona mà không cần triển khai lại**

Người dùng có thể lựa chọn các persona theo từng lĩnh vực. Mỗi persona sẽ cung cấp một system prompt phù hợp với lĩnh vực tương ứng mà không cần sửa hoặc triển khai lại toàn bộ ứng dụng.

**4️⃣ Lưu trữ lịch sử bằng AgentCore Memory**

AgentCore Memory lưu lịch sử hội thoại trong 30 ngày. Nhờ đó, agent có thể tham chiếu đến các lần chỉnh sửa trước mà frontend không cần gửi lại toàn bộ nội dung trò chuyện.

Session ID được lưu trong `localStorage` nên cuộc trò chuyện vẫn tiếp tục sau khi người dùng tải lại trang. Nếu dữ liệu trình duyệt bị xóa, frontend sẽ tạo phiên mới, nhưng lịch sử cũ vẫn có thể được truy xuất từ AgentCore thông qua **ListEvents API**.

**5️⃣ Kết nối công cụ thông qua MCP**

Ba công cụ chỉnh sửa ảnh được xây dựng bằng **AWS Lambda** và cung cấp cho agent thông qua **AgentCore Gateway** cùng **Model Context Protocol (MCP)**.

Agent sẽ dựa vào nội dung yêu cầu để tự lựa chọn đúng công cụ cần sử dụng.

**6️⃣ Xử lý tác vụ không cần AI để tiết kiệm token**

Sau mỗi lần chỉnh sửa, một chương trình Python được chạy trực tiếp trong microVM để thêm watermark. Vì đây là thao tác xử lý ảnh thông thường nên không cần mô hình suy luận và không phát sinh chi phí token.

### Kiến trúc hệ thống

* Một giao diện người dùng **React** được lưu trữ trên **AWS Amplify**, nơi người dùng tải lên hình ảnh, vẽ mặt nạ và nhập hướng dẫn chỉnh sửa.
* Một máy chủ proxy **AWS Lambda** hoạt động như một ranh giới bảo mật giữa thông tin xác thực trình duyệt và API của hệ thống, đồng thời kiểm soát các lời nhắc hệ thống nào được cho phép.
* Một tác nhân **AgentCore** của Amazon Bedrock với **AgentCore Memory** để lưu trữ cuộc hội thoại.
* Ba hàm **Lambda** công cụ gọi các mô hình nền tảng **Stability AI** thông qua Amazon Bedrock để tạo hình ảnh.

Hình dưới đây minh họa kiến trúc tổng quan của giải pháp, từ giao diện người dùng đến các thành phần AgentCore và công cụ xử lý ảnh trên AWS.

![Amazon Bedrock Architecture](/images/3-Blog/bedrock-agentcore-architecture.png)

**Tham khảo:**

https://aws.amazon.com/vi/blogs/machine-learning/build-a-serverless-image-editing-agent-with-amazon-bedrock-agentcore-harness/

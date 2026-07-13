---
title: "Blog 2"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# GỠ LỖI AI AGENTS TRONG MÔI TRƯỜNG PRODUCTION VỚI AMAZON BEDROCK AGENTCORE OBSERVABILITY

Khi các AI Agent ngày càng được triển khai rộng rãi trong môi trường Production, việc giám sát và gỡ lỗi hệ thống trở nên khó khăn hơn rất nhiều so với các ứng dụng truyền thống. Đối với những ứng dụng thông thường, khi xảy ra sự cố hệ thống thường sinh ra Exception, HTTP Status Code hoặc các thông báo lỗi rõ ràng để hỗ trợ quá trình điều tra. Tuy nhiên, AI Agent lại có thể hoàn thành một yêu cầu thành công nhưng vẫn đưa ra câu trả lời sai, lựa chọn công cụ không phù hợp hoặc rơi vào các vòng lặp suy luận mà không hề phát sinh bất kỳ cảnh báo lỗi nào.

Những lỗi "âm thầm" này khiến việc gỡ lỗi trong môi trường Production trở nên phức tạp hơn vì các giải pháp giám sát truyền thống chỉ cho biết rằng Agent đã hoàn thành yêu cầu, nhưng không thể giải thích **tại sao** Agent lại đưa ra quyết định như vậy. Các nhà phát triển thường biết rằng hệ thống đang hoạt động không đúng, nhưng lại thiếu khả năng quan sát toàn bộ quá trình suy luận bên trong của AI Agent.

Amazon Bedrock AgentCore Observability được xây dựng nhằm giải quyết vấn đề này bằng cách cung cấp khả năng quan sát toàn diện mọi giai đoạn trong quá trình thực thi của AI Agent. Thay vì chỉ theo dõi kết quả cuối cùng, dịch vụ cho phép phân tích từng bước suy luận (Reasoning), các lần gọi Tool, quá trình truy xuất Memory, độ trễ, số lượng Token tiêu thụ và toàn bộ Execution Trace. Điều này giúp các nhóm phát triển nhanh chóng xác định nguyên nhân gốc rễ của sự cố và nâng cao độ tin cậy của các ứng dụng AI trong môi trường Production.

Các điểm chính cần nắm:

* Amazon Bedrock AgentCore Observability cung cấp ba lớp giám sát gồm **Metrics**, **Traces** và **Structured Logs**.
* Tích hợp trực tiếp với Amazon CloudWatch để theo dõi thời gian phản hồi, lượng Token sử dụng, số lượng phiên làm việc và tỷ lệ lỗi theo thời gian thực.
* Hỗ trợ OpenTelemetry (OTEL), cho phép xuất dữ liệu giám sát đến Amazon CloudWatch, Grafana, Datadog, Elastic Observability và nhiều nền tảng tương thích khác.
* Cho phép quan sát toàn bộ quá trình suy luận, truy xuất Memory và gọi Tool thay vì chỉ xem kết quả cuối cùng của AI Agent.
* CloudWatch Logs Insights hỗ trợ truy vấn Log mạnh mẽ, giúp nhanh chóng xác định nguyên nhân sự cố và phân tích hành vi thực thi của Agent.
* CloudWatch Alarm có thể tự động phát hiện các bất thường như độ trễ tăng cao, lượng Token tiêu thụ bất thường hoặc tỷ lệ lỗi vượt ngưỡng cho phép.
* AgentCore Observability giúp rút ngắn đáng kể thời gian gỡ lỗi bằng cách cung cấp khả năng quan sát toàn bộ quá trình thực thi của AI Agent.

Theo AWS, các AI Agent trong môi trường Production thường gặp ba nhóm sự cố chính.

Nhóm đầu tiên là **Quality Failures**, xảy ra khi Agent hoàn thành yêu cầu nhưng trả về kết quả không chính xác hoặc gây hiểu nhầm. Những vấn đề phổ biến bao gồm Hallucination, suy luận sai, tính toán không chính xác hoặc lựa chọn Tool không phù hợp với yêu cầu. Do yêu cầu vẫn được xử lý thành công nên các hệ thống giám sát truyền thống thường không phát hiện được những lỗi này.

Nhóm thứ hai là **Reliability Issues**, khiến AI Agent không thể hoàn thành quy trình xử lý. Một số ví dụ phổ biến là lỗi xác thực (401), lỗi phân quyền (403), dữ liệu đầu vào không hợp lệ (400), tài nguyên không tồn tại (404) hoặc mất Session/Context khiến Agent quên toàn bộ lịch sử hội thoại trước đó.

Nhóm cuối cùng là **Efficiency Problems**, chủ yếu ảnh hưởng đến hiệu năng và chi phí vận hành hơn là tính chính xác của kết quả. Những vấn đề thường gặp bao gồm độ trễ cao, lượng Token tiêu thụ quá lớn, gọi cùng một Tool nhiều lần hoặc rơi vào các vòng lặp suy luận (Infinite Reasoning Loop) khiến Agent liên tục tiêu tốn tài nguyên mà không tạo ra kết quả hữu ích.

Một ví dụ thực tế được AWS chia sẻ là trường hợp AI Agent rơi vào **Infinite Reasoning Loop**. Do System Prompt yêu cầu Agent phải tiếp tục thực hiện phép tính cho đến khi đạt được "kết quả hoàn hảo", Agent đã liên tục gọi cùng một Calculator Tool mà không có bất kỳ điều kiện dừng nào.

Trong một phiên làm việc, Agent đã tạo ra hơn **177 Reasoning Spans**, tiêu thụ khoảng **266 nghìn Token** và thực hiện xử lý liên tục trong gần **85 giây**, nhưng hoàn toàn không phát sinh bất kỳ lỗi hệ thống nào. Đối với các công cụ giám sát truyền thống, phiên làm việc này vẫn được ghi nhận là xử lý thành công.

Thông qua Amazon Bedrock AgentCore Observability, nhóm phát triển đã sử dụng OpenTelemetry Traces kết hợp với CloudWatch Logs Insights để phân tích toàn bộ quá trình thực thi. Kết quả cho thấy nguyên nhân không nằm ở Calculator Tool mà xuất phát từ System Prompt được thiết kế chưa hợp lý, thiếu điều kiện dừng trong quá trình suy luận. Sau khi bổ sung giới hạn số bước suy luận và điều kiện kết thúc rõ ràng, vòng lặp vô hạn đã được loại bỏ hoàn toàn.

Ví dụ này cho thấy Execution Traces mang lại góc nhìn chi tiết hơn rất nhiều so với các Application Logs truyền thống. Thay vì chỉ biết rằng hệ thống xảy ra sự cố, các nhà phát triển có thể hiểu chính xác AI Agent đã suy luận như thế nào, đã lựa chọn Tool nào và tại bước nào quá trình thực thi bắt đầu đi chệch khỏi kết quả mong muốn.

Nhìn chung, Amazon Bedrock AgentCore Observability giúp doanh nghiệp vượt xa cách giám sát ứng dụng truyền thống bằng cách cung cấp khả năng quan sát toàn diện hành vi của AI Agent. Thông qua CloudWatch Dashboards, OpenTelemetry Traces, Structured Logs và CloudWatch Logs Insights, các nhóm phát triển có thể chủ động phát hiện sự cố, tối ưu quy trình suy luận, giảm chi phí vận hành và nâng cao độ ổn định của các AI Agent trong môi trường Production.

**Tham khảo:**

https://aws.amazon.com/blogs/machine-learning/debugging-production-agents-with-amazon-bedrock-agentcore-observability/
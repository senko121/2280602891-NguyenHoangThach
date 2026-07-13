---
title: "Blog 3"
date: "2026-07-05"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
 
# WEB SEARCH TRONG AMAZON BEDROCK AGENTCORE

Các **Large Language Models (LLMs)** ngày càng trở thành thành phần cốt lõi trong nhiều ứng dụng AI hiện đại. Tuy nhiên, một hạn chế lớn của các mô hình này là chỉ có thể trả lời dựa trên dữ liệu đã được sử dụng trong quá trình huấn luyện. Khi người dùng đặt các câu hỏi về tin tức mới nhất, giá cổ phiếu theo thời gian thực, các mô hình AI vừa được phát hành hoặc những thông báo mới từ AWS, AI Agent sẽ không thể cung cấp câu trả lời chính xác nếu không có khả năng truy cập thông tin bên ngoài.

Để giải quyết vấn đề này, AWS giới thiệu **Web Search trong Amazon Bedrock AgentCore** – một dịch vụ tìm kiếm web được quản lý hoàn toàn (**Fully Managed**), cho phép AI Agent truy xuất thông tin theo thời gian thực từ Internet mà không cần tích hợp các dịch vụ tìm kiếm của bên thứ ba như Google Search API, Bing Search API hay SerpAPI.

## Các điểm chính cần nắm

* Cho phép AI Agent truy cập thông tin mới nhất từ Internet theo thời gian thực.
* Không cần tích hợp Google Search API, Bing Search API hoặc SerpAPI.
* AWS quản lý toàn bộ quá trình lập chỉ mục, tìm kiếm và xử lý kết quả.
* Hỗ trợ **Model Context Protocol (MCP)** giúp tích hợp dễ dàng với các AI Framework hiện đại.
* Hoạt động thông qua **Amazon Bedrock AgentCore Gateway**.
* Toàn bộ truy vấn của người dùng được xử lý bên trong hạ tầng AWS, giúp nâng cao tính bảo mật và quyền riêng tư.
* Sử dụng **Semantic Snippet Extraction** để chỉ trả về những đoạn nội dung liên quan nhất với truy vấn.
* Tích hợp **Knowledge Graph** nhằm nâng cao độ chính xác cho các câu hỏi liên quan đến thực thể (Entity).

Tính năng này đặc biệt phù hợp với AI Chatbot, trợ lý ảo, Research Agent, Financial Assistant, hệ thống tổng hợp tin tức hoặc bất kỳ ứng dụng AI nào cần sử dụng dữ liệu được cập nhật liên tục theo thời gian thực.

---

## Kiến trúc tổng thể

Quy trình hoạt động của Web Search trong Amazon Bedrock AgentCore gồm các bước sau:

1. Người dùng gửi yêu cầu đến AI Agent.
2. AI Agent xác định rằng câu hỏi cần truy cập dữ liệu mới nhất từ Internet.
3. Yêu cầu được chuyển đến **Amazon Bedrock AgentCore Gateway**.
4. Gateway gọi **Managed Web Search Tool**.
5. AWS tìm kiếm dữ liệu trong **Amazon Web Index** và trích xuất các nội dung phù hợp.
6. Kết quả tìm kiếm được trả về AI Agent.
7. AI Agent tổng hợp thông tin và sinh câu trả lời có kèm nguồn tham khảo (Citation).

![Kiến trúc Web Search](/images/3-Blog/image.png)

---

## Công cụ Web Search (Web Search Tool)

Các nhà phát triển có thể kích hoạt Web Search rất dễ dàng bằng cách gắn **Managed Web Search Tool** vào AgentCore Gateway hiện có thông qua Connector sau:

```python
connectorId = "web-search"
```

Sau khi được cấu hình, AI Agent sẽ tự động quyết định thời điểm sử dụng Web Search khi gặp những câu hỏi yêu cầu dữ liệu theo thời gian thực mà mô hình không thể trả lời chỉ bằng dữ liệu huấn luyện.

---

## Chỉ mục Web của Amazon (Amazon Web Index)

Khác với nhiều giải pháp AI hiện nay phải phụ thuộc vào các công cụ tìm kiếm của bên thứ ba, AWS xây dựng và vận hành **Amazon Web Index** riêng.

Theo AWS, Amazon Web Index có những đặc điểm nổi bật sau:

* Chứa hàng chục tỷ tài liệu trên Internet.
* Dữ liệu được cập nhật liên tục chỉ trong vài phút.
* Bao phủ phạm vi rộng trên Internet công cộng.
* Được tối ưu riêng cho các bài toán AI Retrieval.

Nhờ đó, AI Agent có thể truy cập những thông tin mới nhất với độ tin cậy cao mà không cần phụ thuộc vào các dịch vụ tìm kiếm bên ngoài.

---

## Trích xuất đoạn nội dung ngữ nghĩa (Semantic Snippet Extraction)

Thay vì trả về toàn bộ nội dung của một trang web hoặc mã HTML gốc, Web Search chỉ trích xuất những đoạn văn bản liên quan nhất đến truy vấn của người dùng.

Cách tiếp cận này mang lại nhiều lợi ích như:

* Giảm số lượng Token cần xử lý.
* Cải thiện chất lượng câu trả lời.
* Tăng độ chính xác của quá trình Retrieval.
* Loại bỏ các nội dung không liên quan trên trang web.

Nhờ đó, AI Agent có thể tập trung vào những thông tin quan trọng nhất thay vì phải xử lý toàn bộ nội dung của trang.

---

## Đồ thị tri thức (Knowledge Graph)

Bên cạnh dữ liệu Web thông thường, Web Search còn tích hợp **Knowledge Graph** để hỗ trợ các câu hỏi liên quan đến thực thể (Entity).

Ví dụ:

* Con người (People)
* Tổ chức (Organizations)
* Doanh nghiệp (Companies)
* Địa điểm (Locations)

Knowledge Graph cung cấp dữ liệu có cấu trúc, giúp AI Agent nâng cao độ chính xác, giảm hiện tượng Hallucination và cải thiện độ tin cậy của câu trả lời.

---

## Bảo mật

Một trong những ưu điểm lớn của Amazon Bedrock AgentCore Web Search là kiến trúc ưu tiên bảo mật và quyền riêng tư.

Toàn bộ truy vấn tìm kiếm được xử lý bên trong hạ tầng AWS thay vì chuyển tiếp đến các công cụ tìm kiếm bên ngoài.

Quá trình xác thực được thực hiện thông qua **IAM Role** gắn với AgentCore Gateway, giúp đơn giản hóa việc quản lý quyền truy cập đồng thời đáp ứng các yêu cầu bảo mật của doanh nghiệp.

---

## Tích hợp với các AI Framework

Web Search tuân theo tiêu chuẩn **Model Context Protocol (MCP)**.

Nhờ đó, dịch vụ có thể tích hợp dễ dàng với nhiều AI Framework phổ biến như:

* Strands
* LangChain
* LangGraph
* CrewAI

AI Agent sẽ tự động phát hiện các công cụ (Tools) được Gateway cung cấp và chủ động sử dụng Web Search khi cần truy cập thông tin theo thời gian thực.

---

## Chi phí

Theo bảng giá do AWS công bố:

* **7 USD cho mỗi 1.000 yêu cầu tìm kiếm (Search Requests).**

Dịch vụ áp dụng mô hình **Pay-as-you-go**, cho phép doanh nghiệp chỉ thanh toán dựa trên số lượng yêu cầu thực tế mà hệ thống sử dụng.

---

## Kết luận

Amazon Bedrock AgentCore Web Search giúp AI Agent vượt qua giới hạn của dữ liệu huấn luyện tĩnh bằng cách cung cấp khả năng truy cập thông tin mới nhất từ Internet theo thời gian thực.

Thay vì phải tích hợp nhiều dịch vụ tìm kiếm khác nhau, quản lý API Key hoặc tự xây dựng hệ thống Retrieval riêng, các nhà phát triển chỉ cần cấu hình **Managed Web Search Tool**, trong khi AWS đảm nhiệm toàn bộ quá trình lập chỉ mục, xác thực, tìm kiếm và tối ưu kết quả.

Đây là giải pháp phù hợp cho các AI Chatbot, AI Assistant, nền tảng nghiên cứu, ứng dụng tài chính, hệ thống tổng hợp tin tức và các giải pháp chăm sóc khách hàng cần khai thác dữ liệu luôn được cập nhật theo thời gian thực.

---

## Tham khảo

**AWS Machine Learning Blog**

**Introducing Web Search on Amazon Bedrock AgentCore**

https://aws.amazon.com/blogs/machine-learning/introducing-web-search-on-amazon-bedrock-agentcore/
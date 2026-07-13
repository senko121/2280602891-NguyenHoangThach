---
title: "Blog 1"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# TỐI ƯU CHI PHÍ AI VỚI AMAZON BEDROCK

Khi các ứng dụng Generative AI ngày càng được triển khai rộng rãi, việc kiểm soát chi phí hạ tầng AI đã trở thành một trong những thách thức lớn đối với nhiều doanh nghiệp. Trong giai đoạn phát triển ban đầu, việc sử dụng một Foundation Model duy nhất thường đủ để đáp ứng nhu cầu thử nghiệm. Tuy nhiên, khi hệ thống được đưa vào môi trường Production và phải xử lý hàng nghìn yêu cầu mỗi ngày, chi phí sử dụng AI có thể tăng rất nhanh và trở thành một khoản vận hành đáng kể.

Bên cạnh vấn đề chi phí, nhiều doanh nghiệp còn gặp phải tình trạng **vendor lock-in** khi phụ thuộc hoàn toàn vào một nhà cung cấp AI duy nhất. Điều này khiến việc đánh giá các mô hình mới, tối ưu chi phí hoặc lựa chọn mô hình phù hợp cho từng nghiệp vụ trở nên khó khăn hơn khi công nghệ AI liên tục phát triển.

Amazon Bedrock được xây dựng nhằm giải quyết những vấn đề này bằng cách cung cấp một nền tảng thống nhất cho phép truy cập nhiều Foundation Model thông qua cùng một API. Thay vì sử dụng một mô hình cho tất cả các tác vụ, doanh nghiệp có thể lựa chọn Foundation Model phù hợp nhất dựa trên yêu cầu về khả năng suy luận, tốc độ phản hồi, hiệu năng và chi phí vận hành. Cách tiếp cận Multi-Model này giúp tối ưu tài nguyên, nâng cao hiệu quả xử lý và duy trì khả năng mở rộng của hệ thống.

Các điểm chính cần nắm:

* Amazon Bedrock cung cấp quyền truy cập đến nhiều Foundation Model như Amazon Nova, Anthropic Claude, Meta Llama, Cohere Command, Amazon Titan và nhiều mô hình khác thông qua một API thống nhất.
* Mỗi tác vụ AI có thể sử dụng Foundation Model khác nhau nhằm cân bằng giữa khả năng suy luận, tốc độ xử lý, hiệu năng và chi phí vận hành.
* Prompt Caching giúp giảm việc xử lý lặp lại các Prompt, từ đó giảm số lượng token tiêu thụ và cải thiện thời gian phản hồi.
* Intelligent Prompt Routing tự động lựa chọn Foundation Model có chi phí tối ưu nhất cho từng yêu cầu mà không cần thay đổi logic của ứng dụng.
* Prompt Optimization hỗ trợ tối ưu Prompt cho từng Foundation Model nhằm nâng cao chất lượng phản hồi và giảm lượng token không cần thiết.
* Kiến trúc Multi-Model giúp hạn chế tình trạng vendor lock-in, đồng thời tạo điều kiện thuận lợi để tích hợp các Foundation Model mới trong tương lai.
* Doanh nghiệp có thể tối ưu chi phí hạ tầng AI mà vẫn đảm bảo hiệu năng, khả năng mở rộng và tính sẵn sàng của hệ thống.

Một ví dụ thực tế được AWS chia sẻ là **InterWiz**, nền tảng AI hỗ trợ phỏng vấn tuyển dụng với hàng nghìn cuộc phỏng vấn được thực hiện mỗi tháng. Ban đầu, hệ thống sử dụng chủ yếu một Foundation Model duy nhất, dẫn đến chi phí vận hành ngày càng tăng, độ trễ cao và khả năng mở rộng bị hạn chế khi số lượng người dùng tăng lên.

Để giải quyết vấn đề này, InterWiz đã chuyển toàn bộ hệ thống AI sang Amazon Bedrock với kiến trúc Multi-Model. Thay vì giao toàn bộ công việc cho một Foundation Model, từng mô hình được lựa chọn theo thế mạnh riêng. Anthropic Claude 3.5 Sonnet được sử dụng cho các tác vụ yêu cầu khả năng suy luận và tạo câu hỏi chất lượng cao, trong khi Meta Llama 3.3 70B đảm nhiệm các cuộc hội thoại thời gian thực và đánh giá ứng viên với chi phí thấp hơn cùng tốc độ xử lý nhanh hơn.

Trong quá trình chuyển đổi, nhóm phát triển cũng tối ưu Prompt cho từng Foundation Model, áp dụng Prompt Caching nhằm giảm lượng xử lý lặp lại và sử dụng Intelligent Prompt Routing để tự động lựa chọn mô hình có chi phí phù hợp nhất cho từng loại yêu cầu. Nhờ đó, hệ thống vừa cải thiện hiệu năng kỹ thuật vừa tối ưu đáng kể chi phí vận hành.

Kết quả đạt được sau quá trình triển khai:

* Giảm **90%** chi phí hạ tầng AI.
* Cải thiện **55%** thời gian phản hồi (từ khoảng **850 ms** xuống còn **450 ms**).
* Duy trì **99,9%** tính sẵn sàng của dịch vụ nhờ kiến trúc Multi-Model kết hợp cơ chế tự động chuyển đổi khi xảy ra sự cố.
* Tăng tính linh hoạt của hệ thống, giúp doanh nghiệp dễ dàng đánh giá và tích hợp các Foundation Model mới mà không cần thay đổi toàn bộ kiến trúc ứng dụng.

Qua trường hợp của InterWiz có thể thấy rằng việc tối ưu hạ tầng AI không chỉ đơn thuần là lựa chọn Foundation Model mạnh nhất. Thay vào đó, việc sử dụng đúng mô hình cho từng nghiệp vụ sẽ giúp cân bằng tốt hơn giữa hiệu năng, chi phí vận hành, khả năng mở rộng và tính linh hoạt trong tương lai. Amazon Bedrock cung cấp nền tảng phù hợp để hiện thực hóa chiến lược này thông qua hệ sinh thái Foundation Model đa dạng cùng một API thống nhất.

Hình dưới đây minh họa kiến trúc tổng quan của Amazon Bedrock, cho phép doanh nghiệp xây dựng hệ thống AI theo mô hình Multi-Model nhằm tối ưu chi phí mà vẫn đảm bảo hiệu năng và khả năng mở rộng.

![Amazon Bedrock Architecture](/images/3-Blog/bedrock-architecture.png)

**Tham khảo:**

https://aws.amazon.com/blogs/apn/how-interwiz-reduced-ai-costs-by-90-percent-with-amazon-bedrock/
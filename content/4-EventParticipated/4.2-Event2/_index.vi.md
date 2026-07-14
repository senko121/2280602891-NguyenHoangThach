---
title: "Event 2"
date: 2026-05-12
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch: "FCAJ Community Day"

### Mục đích của sự kiện

* Cập nhật góc nhìn thực tế về thị trường việc làm ngành công nghệ trong thời đại AI.
* Chia sẻ các kỹ thuật làm việc hiệu quả với LLM: quản lý ngữ cảnh (Context Engineering) và hiểu rõ giới hạn của mô hình.
* Giới thiệu các dịch vụ AWS hiện đại: Amazon Quick Suite và Amazon CloudFront trong kiến trúc hệ thống.
* Truyền cảm hứng qua kinh nghiệm thi đấu hackathon và xây dựng hệ thống multi-agent cấp doanh nghiệp.

### Danh sách diễn giả

* **Anh Nguyễn Gia Hưng** – Chia sẻ góc nhìn về thị trường việc làm và AI.
* **Anh Tịnh Trương** (Platform Engineer) – Chủ đề "Context is Everything" về quản lý ngữ cảnh khi làm việc với LLM.
* **Anh Nguyễn Hải Anh** – Chủ đề "Friendly AI Assistant with Amazon Quick" giới thiệu trợ lý AI Amazon Quick Suite.
* **Anh Nguyễn Tuấn Thịnh** – Chủ đề "CloudFront as Your Foundation" về vai trò nền tảng của Amazon CloudFront.
* **Nhóm UTM Morpho (chị Uyển Thảo)** – Chia sẻ kinh nghiệm tham gia hackathon.
* **Anh Đức Đào** – Chủ đề "Non-determinism of 'Deterministic' LLM Settings" về tính bất định của LLM.
* **Chị Vy Lâm** – Chủ đề "Enterprise-grade Multi-agent System" về hệ thống đa tác tử cấp doanh nghiệp.

### Nội dung nổi bật

#### Góc nhìn về thị trường việc làm và AI – Anh Nguyễn Gia Hưng

Phần mở đầu mang đến bức tranh tổng quan về thị trường tuyển dụng ngành công nghệ khi AI phát triển mạnh mẽ.

* AI không thay thế hoàn toàn lập trình viên nhưng đang thay đổi tiêu chuẩn tuyển dụng: doanh nghiệp kỳ vọng ứng viên biết sử dụng AI để tăng năng suất.
* Các vị trí Junior chịu áp lực cạnh tranh lớn hơn, vì vậy kiến thức nền tảng vững chắc và khả năng học nhanh trở thành lợi thế quan trọng nhất.
* Kỹ năng mềm, tư duy giải quyết vấn đề và khả năng thích nghi là những yếu tố AI chưa thể thay thế.
* Lời khuyên cho sinh viên: xây dựng portfolio dự án thực tế thay vì chỉ tích lũy chứng chỉ.

#### Context is Everything – Anh Tịnh Trương (Platform Engineer)

Phần trình bày tập trung vào Context Engineering — yếu tố quyết định chất lượng đầu ra khi làm việc với LLM.

* Chất lượng phản hồi của LLM phụ thuộc trực tiếp vào chất lượng ngữ cảnh được cung cấp, không chỉ vào cách viết Prompt.
* Cửa sổ ngữ cảnh (context window) là tài nguyên hữu hạn: cần chọn lọc thông tin đưa vào thay vì nhồi nhét mọi thứ.
* Các kỹ thuật quản lý ngữ cảnh: tóm tắt lịch sử hội thoại, truy xuất tài liệu liên quan (RAG), và cấu trúc hóa thông tin đầu vào.
* Với vai trò Platform Engineer, việc thiết kế hệ thống cung cấp đúng ngữ cảnh cho AI quan trọng hơn việc tinh chỉnh từng câu Prompt.

#### Friendly AI Assistant with Amazon Quick – Anh Nguyễn Hải Anh

Phần chia sẻ giới thiệu **Amazon Quick Suite** — ứng dụng AI agentic mới của AWS dành cho môi trường làm việc.

* Amazon Quick Suite giúp tìm kiếm thông tin, nghiên cứu chuyên sâu, trực quan hóa dữ liệu và tự động hóa tác vụ trong công việc hằng ngày.
* Khả năng kết nối với nhiều nguồn dữ liệu của doanh nghiệp giúp trợ lý AI trả lời dựa trên dữ liệu nội bộ thay vì chỉ kiến thức tổng quát.
* Demo trực tiếp cách xây dựng một trợ lý AI thân thiện phục vụ nhu cầu tra cứu và báo cáo của các team không chuyên về kỹ thuật.
* Điểm nhấn: AI assistant hiệu quả không cần phức tạp — quan trọng là tích hợp đúng vào quy trình làm việc hiện có.

#### CloudFront as Your Foundation – Anh Nguyễn Tuấn Thịnh

Phần trình bày phân tích vai trò của **Amazon CloudFront** như lớp nền tảng của kiến trúc web hiện đại.

* CloudFront không chỉ là CDN tăng tốc nội dung mà còn là cổng vào (entry point) bảo mật cho toàn hệ thống.
* Kết hợp CloudFront với S3 (qua Origin Access Control) và ALB giúp phục vụ cả nội dung tĩnh lẫn API qua một domain HTTPS duy nhất.
* Các lợi ích nền tảng: chống DDoS ở tầng edge, tích hợp WAF, cache giảm tải cho origin và tối ưu chi phí truyền dữ liệu.
* Bài học kiến trúc: đặt CloudFront làm nền móng ngay từ đầu giúp hệ thống dễ mở rộng và bảo mật hơn về sau — rất gần với kiến trúc em áp dụng trong workshop PetShop.

#### Chia sẻ kinh nghiệm hackathon – Nhóm UTM Morpho (chị Uyển Thảo)

Phần chia sẻ mang đến trải nghiệm thực tế của nhóm sinh viên khi tham gia các cuộc thi hackathon.

* Quy trình làm việc trong 24–48 giờ: chọn ý tưởng nhanh, phân công theo thế mạnh và ưu tiên sản phẩm demo được.
* Bài học về teamwork dưới áp lực thời gian: giao tiếp ngắn gọn, quyết định dứt khoát và chấp nhận đánh đổi phạm vi tính năng.
* Cách trình bày (pitching) trước ban giám khảo: tập trung vào vấn đề giải quyết được thay vì liệt kê công nghệ sử dụng.
* Hackathon là cơ hội học nhanh nhất: thất bại trong cuộc thi cũng mang lại kinh nghiệm và kết nối giá trị.

#### Non-determinism of "Deterministic" LLM Settings – Anh Đức Đào

Phần trình bày kỹ thuật chuyên sâu lý giải vì sao LLM vẫn cho kết quả khác nhau ngay cả khi cấu hình "deterministic".

* Đặt `temperature = 0` không đảm bảo đầu ra giống nhau tuyệt đối giữa các lần gọi.
* Nguyên nhân đến từ tầng hạ tầng: phép toán dấu phẩy động không kết hợp (floating-point non-associativity), thứ tự tính toán song song trên GPU và cơ chế gom batch của hệ thống phục vụ mô hình.
* Hệ quả thực tế: không nên thiết kế hệ thống phụ thuộc vào việc LLM trả về kết quả y hệt nhau; cần validation và xử lý sai khác ở tầng ứng dụng.
* Bài học: hiểu giới hạn của công cụ quan trọng không kém việc biết sử dụng công cụ.

#### Enterprise-grade Multi-agent System – Chị Vy Lâm

Phần cuối giới thiệu cách xây dựng hệ thống nhiều AI agent phối hợp đạt chuẩn triển khai doanh nghiệp.

* Kiến trúc multi-agent: agent điều phối (orchestrator) phân chia nhiệm vụ cho các agent chuyên biệt thay vì một agent làm mọi việc.
* Các yêu cầu cấp doanh nghiệp: kiểm soát quyền truy cập, ghi log đầy đủ (observability), giới hạn chi phí và cơ chế con người phê duyệt (human-in-the-loop).
* Thách thức thực tế: quản lý ngữ cảnh chia sẻ giữa các agent, xử lý lỗi dây chuyền và đánh giá chất lượng đầu ra tự động.
* Xu hướng: multi-agent system là bước tiến từ chatbot đơn lẻ sang quy trình nghiệp vụ tự động hóa đầu-cuối.

### Những gì học được

#### Định hướng nghề nghiệp

* Thị trường việc làm thay đổi nhanh nhưng kiến thức nền tảng và khả năng thích nghi vẫn là giá trị cốt lõi.
* Biết sử dụng AI hiệu quả đã trở thành kỹ năng mặc định mà nhà tuyển dụng kỳ vọng.
* Portfolio dự án thực tế có sức thuyết phục hơn danh sách chứng chỉ.

#### Tư duy làm việc với AI

* Quản lý ngữ cảnh (Context Engineering) quyết định chất lượng đầu ra nhiều hơn cách viết Prompt đơn lẻ.
* LLM có tính bất định ngay cả ở cấu hình "deterministic" — hệ thống thực tế phải có cơ chế kiểm tra và xử lý sai khác.
* Hệ thống multi-agent cần các ràng buộc doanh nghiệp: bảo mật, giám sát, kiểm soát chi phí và sự phê duyệt của con người.

#### Kiến trúc hệ thống trên AWS

* CloudFront nên được xem là lớp nền tảng của kiến trúc web: vừa tăng tốc, vừa bảo mật, vừa hợp nhất điểm truy cập.
* Amazon Quick Suite mở ra hướng xây dựng trợ lý AI nội bộ kết nối trực tiếp với dữ liệu doanh nghiệp.

#### Kỹ năng mềm

* Làm việc nhóm dưới áp lực thời gian đòi hỏi giao tiếp ngắn gọn và quyết định nhanh.
* Kỹ năng trình bày (pitching) tập trung vào giá trị giải quyết vấn đề thay vì chi tiết công nghệ.

### Ứng dụng vào công việc

* Áp dụng tư duy **Context Engineering** khi làm việc với AI: chuẩn bị tài liệu và ngữ cảnh trước khi đặt câu hỏi thay vì chỉ chỉnh sửa Prompt.
* Củng cố kiến trúc **CloudFront làm cổng vào duy nhất** trong dự án PetShop và các dự án tương lai.
* Thêm bước **validation đầu ra của LLM** trong các ứng dụng có tích hợp AI, không giả định kết quả luôn nhất quán.
* Tìm hiểu và thử nghiệm **Amazon Quick Suite** cho các nhu cầu tra cứu và báo cáo tự động.
* Tham gia hackathon để rèn kỹ năng làm việc nhóm, quản lý thời gian và trình bày sản phẩm.

### Trải nghiệm trong sự kiện

Tham gia sự kiện **"FCAJ Community Day"** giúp em tiếp cận nhiều chủ đề đa dạng — từ định hướng nghề nghiệp, kỹ thuật chuyên sâu về LLM cho đến kiến trúc hệ thống trên AWS.

#### Học hỏi từ các diễn giả giàu kinh nghiệm

Bảy phần trình bày đến từ các diễn giả với vai trò khác nhau — Platform Engineer, kỹ sư AI, kiến trúc sư giải pháp và cả nhóm sinh viên thi đấu hackathon — mang lại góc nhìn đa chiều về ngành công nghệ.

Sự kết hợp giữa chủ đề định hướng và chủ đề kỹ thuật giúp em vừa mở rộng tầm nhìn vừa học được kiến thức áp dụng ngay được.

#### Hiểu sâu hơn về bản chất của LLM

Hai phần trình bày về Context Engineering và tính bất định của LLM giúp em thay đổi cách nhìn về AI: thay vì coi AI là hộp đen luôn đúng, em học được cách hiểu giới hạn của mô hình và thiết kế quy trình làm việc phù hợp.

#### Kết nối kiến thức với thực hành

Phần trình bày về CloudFront đặc biệt giá trị vì trùng khớp với kiến trúc em đang triển khai trong workshop PetShop — nghe phân tích từ góc nhìn chuyên gia giúp em hiểu rõ hơn các quyết định kiến trúc mình đã áp dụng.

#### Giao lưu và trao đổi

Sự kiện tạo cơ hội trao đổi trực tiếp với các diễn giả và các bạn tham dự, đặc biệt là phần chia sẻ hackathon truyền cảm hứng để em mạnh dạn tham gia các cuộc thi công nghệ sắp tới.

#### Bài học rút ra

* Ngữ cảnh là yếu tố quan trọng nhất khi làm việc với LLM — "Context is Everything".
* Hiểu giới hạn của công nghệ quan trọng không kém việc biết sử dụng công nghệ.
* Kiến trúc tốt bắt đầu từ nền móng đúng — CloudFront là ví dụ điển hình cho tư duy này.
* Thị trường việc làm thời AI ưu tiên người có nền tảng vững và khả năng học nhanh.
* Trải nghiệm thực chiến (hackathon, dự án thực tế) là cách học hiệu quả và thuyết phục nhất.

> Nhìn chung, sự kiện mang lại cho em cả kiến thức kỹ thuật chuyên sâu lẫn định hướng nghề nghiệp rõ ràng hơn trong thời đại AI. Những chia sẻ về Context Engineering, giới hạn của LLM và kiến trúc AWS đều có thể áp dụng trực tiếp vào quá trình học tập và các dự án thực tế của em, đồng thời phần chia sẻ về thị trường việc làm và hackathon giúp em tự tin hơn trên con đường phát triển sự nghiệp trong lĩnh vực công nghệ thông tin.

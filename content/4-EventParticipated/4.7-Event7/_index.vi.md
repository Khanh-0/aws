---
title: "CloudFront as Your Foundation và AWS WAF & Application Protection"
date: "2025-11-18T08:30:00+07:00"
weight: 7
chapter: false
pre: " <b> 4.7. </b> "
---

### Mục tiêu Sự kiện

- Giới thiệu cho người tham dự các khái niệm nền tảng của Amazon CloudFront như một CDN toàn cầu
- Cung cấp kiến thức thực tiễn về cách CloudFront cải thiện hiệu năng, độ tin cậy và tối ưu chi phí
- Tìm hiểu AWS WAF, Shield và Bot Control trong bảo vệ ứng dụng Layer 7
- Minh họa các kiến trúc bảo mật hiện đại cho web applications và API
- Tăng cường học tập thông qua phần Fun Quiz tương tác
- Củng cố kiến thức để xây dựng ứng dụng đám mây an toàn, có khả năng mở rộng và tối ưu chi phí

### Diễn giả

- **Anh Hùng Gia – phiên CloudFront**
- **Mr. Julian – phiên WAF**

### Điểm nổi bật

#### Tăng tốc ứng dụng với Amazon CloudFront

Phiên đầu giới thiệu về những thách thức trong hiệu năng web, chi phí khó dự đoán và lưu lượng tăng đột biến. Người tham dự được tìm hiểu cách CloudFront giải quyết các vấn đề này thông qua:

- Mạng lưới Edge toàn cầu với các Points of Presence
- Multi-layer caching và Origin Shield
- Các kỹ thuật tăng tốc nâng cao (HTTP/3, persistent connections, multiplexing)
- Bảo vệ origin qua VPC Origins và Origin Access Control
- Các tính năng tối ưu chi phí tích hợp sẵn

Các ví dụ thực tế minh họa cách CloudFront giảm độ trễ, giảm tải cho origin, giảm chi phí và cải thiện trải nghiệm người dùng ở quy mô lớn.

---

#### Tăng cường bảo mật với AWS WAF & Shield

Phần thứ hai tập trung vào bảo mật ứng dụng trong bối cảnh mối đe dọa hiện đại, bao gồm:

- Các dạng tấn công phổ biến: OWASP Top 10, DDoS, bot, CVEs
- Các thành phần của AWS WAF (WebACL, rules, rule groups, COUNT mode, rate-based rules)
- AWS Managed Rules cho bảo vệ nhanh chóng
- Bot Control và kỹ thuật phân tích client
- AWS Shield và hệ thống phòng vệ DDoS nhiều lớp
- Các kiến trúc bảo mật CloudFront + WAF + Shield cho bảo vệ L7 vững chắc

Người tham dự hiểu sâu hơn cách xây dựng hệ thống ứng dụng an toàn, resilent bằng các dịch vụ edge của AWS.

---

### Agenda

#### **Phiên Sáng (CloudFront)**

**8:30 – 9:00 AM** | Chào mừng & Giới thiệu

- Tổng quan về các vấn đề hiệu năng của ứng dụng web hiện đại
- Vai trò của CDN trong phân phối toàn cầu

**9:00 – 10:30 AM** | _Amazon CloudFront Deep Dive_

- Hành vi caching và tối ưu cache
- Tổng quan Global Edge Network
- Các cải tiến hiệu năng (HTTP/3, nén, persistent TCP)
- Chiến lược bảo vệ origin
- Failover và multi-origin routing
- **Demo:** Tạo CloudFront distribution & phân tích hành vi

**10:30 – 10:45 AM** | Nghỉ giải lao

---

#### **Phiên Trưa (WAF & Shield)**

**10:45 AM – 12:00 PM** | _AWS WAF & Application Protection_

- Các loại rule và best practices
- Rate-based rules chống HTTP flood
- Tổng quan AWS Managed Rules
- Bot Control và phân tích hành vi client
- Shield Advanced và khả năng phòng vệ tự động
- **Demo:** Tạo WebACL cho CloudFront

**12:00 – 1:00 PM** | Nghỉ trưa (tự túc)

---

#### **Phiên Chiều (Security Architecture & Fun Quiz)**

**1:00 – 2:30 PM** | _Security Architecture at the Edge_

- Tích hợp CloudFront + WAF + Shield
- Origin cloaking và VPC Origins
- Ứng dụng security layers cho API & web apps
- Công cụ quan sát và giám sát bảo mật

**2:30 – 2:45 PM** | Nghỉ giải lao

**2:45 – 3:30 PM** | _Fun Quiz – CloudFront & WAF Challenge_

- Các câu hỏi tình huống thực tế
- Kiểm tra kiến thức về caching, rules và protection layers
- Quiz tương tác theo đội

**3:30 – 4:00 PM** | Tổng kết & Q&A

- Định hướng nghề nghiệp cho mảng AWS Edge & Security
- Lộ trình học CloudFront, WAF và AWS Security

---

### Những Điều Rút Ra

Tôi được chứng kiến một người làm kỹ thuật gần 60 tuổi – Mr. Julian – và người “sifu” tuyệt vời của chúng tôi – anh Hùng Gia.

#### Nhận thức Chiến lược về Edge Performance & Security

- CloudFront cải thiện hiệu năng toàn cầu thông qua caching và tối ưu mạng
- Origin có thể được bảo vệ khỏi truy cập công khai bằng VPC Origins và OAC
- WAF cung cấp khả năng bảo vệ L7 dựa trên rule cực kỳ linh hoạt
- Bot Control nâng cao khả năng phát hiện bot thông qua telemetry và behavioral tokens
- Shield mang lại khả năng chống DDoS tự động ngay tại edge

#### Bài học thực tiễn từ CloudFront

- Multi-layer caching giảm chi phí origin và cải thiện độ trễ
- Failover và routing giúp tăng tính sẵn sàng của ứng dụng
- HTTP/3 và nén nâng cao cải thiện tốc độ đáng kể

#### Bài học thực tiễn từ WAF

- COUNT mode rất quan trọng trước khi bật chế độ chặn
- Rate-based rules hiệu quả trong giảm thiểu flood traffic
- Managed Rules giúp triển khai bảo mật nhanh chóng
- Client interrogation phát hiện các loại bot tinh vi

---

### Ứng dụng vào Công Việc

- Sử dụng CloudFront để giảm độ trễ và giảm tải origin
- Bảo vệ ứng dụng web và API bằng WAF + Managed Rules
- Triển khai Shield để chống DDoS nâng cao
- Áp dụng chiến lược defense-in-depth cho edge và application layers
- Liên tục theo dõi hành vi người dùng và điều chỉnh ruleset

---

### Trải nghiệm Sự kiện

Workshop **CloudFront, WAF & Security Essentials** mang đến cả chiều sâu lý thuyết và thực hành. Tôi hiểu rõ hơn cách doanh nghiệp xây dựng ứng dụng an toàn, hiệu năng cao, tối ưu chi phí ở quy mô toàn cầu.

#### Học hỏi từ Chuyên gia AWS Edge & Security

- Hiểu cách CloudFront tối ưu hiệu năng cho hàng triệu người dùng
- Nắm cách WAF và Shield giảm thiểu tấn công thực tế
- Quan sát nhiều kiến trúc được doanh nghiệp sử dụng

#### Thực hành trực tiếp

- Cấu hình CloudFront distribution và behaviors
- Tạo và kiểm thử WAF rules với tình huống thực tế
- Khám phá bot detection và telemetry-based defense

#### Giao lưu & Kết nối cộng đồng

- Tương tác với những người chung đam mê bảo mật & performance
- Thảo luận các use case thực tế và định hướng nghề nghiệp

#### Bài học then chốt

- Performance và security phải được thiết kế song song tại edge
- Chiến lược caching đúng giúp giảm cả latency lẫn chi phí
- Defense-in-depth với CloudFront + WAF + Shield mang lại lớp bảo vệ vững chắc
- Cần học liên tục để theo kịp sự thay đổi của bối cảnh bảo mật

<div style="text-align: center;">
  {{< figure src="/images/Events/event7.1.jpg">}}
</div>

<div style="text-align: center;">
  {{< figure src="/images/Events/event7.2.jpg">}}
</div>

> Nhìn chung, workshop này đã củng cố hiểu biết của tôi về edge networking, các best practices bảo mật và công cụ bảo vệ ứng dụng AWS—giúp tôi tự tin hơn khi thiết kế hệ thống an toàn, có khả năng mở rộng với CloudFront và AWS WAF.

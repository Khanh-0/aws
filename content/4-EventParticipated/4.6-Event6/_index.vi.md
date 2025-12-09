---
title: "AWS Cloud Mastery Series #2: DevOps on AWS"
date: "2025-11-17T08:30:00+07:00"
weight: 6
chapter: false
pre: " <b> 4.6. </b> "
---

### Mục tiêu Sự kiện

- Giới thiệu cho người tham dự về các nguyên tắc và văn hoá DevOps hiện đại
- Cung cấp kiến thức thực tiễn về các công cụ DevOps trên AWS trong CI/CD, hạ tầng và vận hành
- Minh họa cách DevOps tăng tốc độ triển khai thông qua tự động hóa, quan sát hệ thống, và cải tiến liên tục
- Trang bị hiểu biết thực hành về containerization, monitoring và chiến lược deploy
- Củng cố kiến thức về độ tin cậy, khả năng mở rộng, và operational excellence trên AWS
- Hỗ trợ người tham dự khám phá định hướng nghề nghiệp DevOps và các chứng chỉ AWS

### Diễn giả

- **Đội ngũ AWS Vietnam DevOps Specialist**
- **Khách mời từ AWS Training & Certification**

### Điểm nổi bật

#### Tiếp cận Tư Duy DevOps

Workshop mở đầu bằng thảo luận về sự chuyển dịch mạnh mẽ sang văn hóa DevOps. Người tham dự được giới thiệu:

- Các nguyên tắc hợp tác của DevOps
- Bộ chỉ số hiệu suất quan trọng (DORA metrics: Deployment Frequency, MTTR, Change Failure Rate)
- Tầm quan trọng của tự động hóa và cải tiến liên tục
- Cách DevOps bổ trợ cho quy trình AI/ML từ buổi trước

Phiên này nhấn mạnh rằng văn hóa DevOps mạnh giúp tăng tốc độ triển khai, nâng cao độ tin cậy và củng cố tinh thần ownership trong kỹ thuật.

---

#### Tìm Hiểu Sâu về Bộ Công Cụ DevOps trên AWS

Trong suốt ngày workshop, người tham dự khám phá các công cụ AWS-native về CI/CD, Infrastructure as Code, container orchestration và observability—hiểu rõ cách các đội kỹ thuật hiện đại xây dựng và triển khai hệ thống ở quy mô lớn.

---

### Agenda

#### **Phiên Sáng (8:30 AM – 12:00 PM)**

**8:30 – 9:00 AM** | Welcome & DevOps Mindset

- Tóm tắt buổi AI/ML trước
- Văn hóa và nguyên tắc DevOps
- Lợi ích và các chỉ số chính (DORA, MTTR, deployment frequency)

**9:00 – 10:30 AM** | _AWS DevOps Services – CI/CD Pipeline_

- Source Control: AWS CodeCommit, GitFlow, Trunk-based development
- Build & Test: cấu hình CodeBuild, kiểm thử tự động
- Deployment: CodeDeploy với Blue/Green, Canary, Rolling updates
- Orchestration: tự động hóa workflow bằng CodePipeline
- **Demo:** Walkthrough pipeline CI/CD hoàn chỉnh

**10:30 – 10:45 AM** | Nghỉ giải lao

**10:45 AM – 12:00 PM** | _Infrastructure as Code (IaC)_

- AWS CloudFormation: templates, stacks, drift detection
- AWS CDK: constructs, reusable patterns, multi-language
- **Demo:** Triển khai hạ tầng bằng CloudFormation và CDK
- Thảo luận: Chọn công cụ IaC nào cho từng trường hợp

**12:00 – 1:00 PM** | Nghỉ trưa (tự túc)

---

#### **Phiên Chiều (1:00 PM – 5:00 PM)**

**1:00 – 2:30 PM** | _Container Services on AWS_

- Kiến thức cơ bản về Docker: microservices và containerization
- Amazon ECR: lưu trữ image, scanning, lifecycle policy
- Amazon ECS & EKS: chiến lược deploy, scaling, orchestration
- AWS App Runner: deploy container đơn giản
- **Demo & Case Study:** So sánh triển khai microservices

**2:30 – 2:45 PM** | Nghỉ giải lao

**2:45 – 4:00 PM** | _Monitoring & Observability_

- CloudWatch: metrics, logs, alarms, dashboards
- AWS X-Ray: distributed tracing & insights hiệu năng
- **Demo:** Thiết lập full-stack observability
- Best practices: alerting, dashboards, quy trình on-call

**4:00 – 4:45 PM** | _DevOps Best Practices & Case Studies_

- Mô hình deploy: feature flags, A/B testing
- Kiểm thử tự động & tích hợp CI/CD
- Quản lý sự cố và postmortem
- Case studies từ startups & enterprise

**4:45 – 5:00 PM** | Q&A & Wrap-up

- Định hướng nghề nghiệp DevOps
- Lộ trình chứng chỉ AWS

---

### Những Điều Rút Ra

#### Nhận Thức Chiến Lược về DevOps trên AWS

- DevOps tăng tốc triển khai và cải thiện hợp tác nhờ tự động hóa
- CI/CD giảm rủi ro và tăng tần suất release
- IaC đảm bảo hạ tầng nhất quán, có thể tái sử dụng và dễ mở rộng
- Containers tiêu chuẩn hóa môi trường triển khai
- Observability cực kỳ quan trọng cho high availability

#### Bài học Thực tiễn từ CI/CD

- CodeCommit, CodeBuild, CodeDeploy và CodePipeline tạo thành pipeline tự động hoàn chỉnh
- Chiến lược deploy như Canary, Blue/Green giảm downtime và rủi ro
- Git strategies như Trunk-based development tăng tốc delivery

#### Bài học Thực tiễn từ IaC

- CloudFormation mang lại cách quản lý hạ tầng có kiểm soát và khai báo
- CDK giúp mô hình hóa hạ tầng linh hoạt bằng code
- Hiểu khi nào dùng CloudFormation, khi nào dùng CDK là chìa khóa cho doanh nghiệp

#### Bài học Thực tiễn từ Containers & Observability

- ECS và EKS mang lại orchestration mạnh mẽ cho microservices
- App Runner đơn giản hóa quy trình deploy
- CloudWatch + X-Ray giúp quan sát toàn bộ hệ thống

---

### Ứng dụng vào Công Việc

- Xây dựng pipeline CI/CD để tự động hoá build, test và deploy
- Sử dụng IaC để duy trì môi trường hạ tầng nhất quán
- Áp dụng containers cho ứng dụng scalable và portable
- Áp dụng thực hành observability để tăng độ tin cậy và giảm MTTR
- Tiếp tục học các công cụ DevOps và theo đuổi chứng chỉ AWS

---

### Trải nghiệm Sự kiện

Workshop **DevOps on AWS** đem lại cái nhìn toàn diện và thực tế về quy trình DevOps hiện đại, tạo nền tảng vững chắc cho thử thách kỹ thuật thực tế.

#### Học hỏi từ Chuyên gia AWS DevOps

- Hiểu sâu về bộ công cụ DevOps native của AWS
- Nắm cách doanh nghiệp triển khai chuyển đổi DevOps ở quy mô lớn
- Hiểu vai trò thực sự của automation và observability trong production

#### Thực hành Trực tiếp

- Quan sát pipeline CI/CD vận hành
- Thấy cách IaC đơn giản hóa provisioning và maintenance
- So sánh các chiến lược deploy containers
- Tạo dashboard monitoring và phân tích distributed tracing

#### Giao lưu & Kết nối

- Gặp gỡ nhiều kỹ sư và developer đam mê DevOps
- Chia sẻ tài nguyên, kinh nghiệm và định hướng chứng chỉ
- Mở rộng network trong cộng đồng AWS DevOps

#### Bài học Quan trọng

- DevOps là sự kết hợp của văn hóa, công cụ và kỷ luật vận hành
- Automation là chìa khóa cho tốc độ, độ tin cậy và hiệu quả
- Observability phải được tích hợp ngay từ đầu
- Cần học liên tục trong môi trường DevOps thay đổi nhanh

<div style="text-align: center;">
  {{< figure src="/images/Events/event6.1.jpg">}}
</div>

<div style="text-align: center;">
  {{< figure src="/images/Events/event6.2.jpg">}}
</div>

<div style="text-align: center;">
  {{< figure src="/images/Events/event6.3.jpg">}}
</div>

<div style="text-align: center;">
  {{< figure src="/images/Events/event6.4.jpg">}}
</div>

<div style="text-align: center;">
  {{< figure src="/images/Events/event6.5.jpg">}}
</div>

> Nhìn chung, phiên học này đã củng cố hiểu biết của tôi về nền tảng DevOps, bộ công cụ AWS và các best practices—giúp tôi tự tin hơn trong việc thiết kế, tự động hóa và vận hành các hệ thống cloud-native hiện đại.

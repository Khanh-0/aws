---
title: "Proposal"
date: "2025-10-28"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# APT Magic  
## Nền tảng AI Serverless cho Tạo Ảnh Cá Nhân Hóa và Tương Tác Xã Hội

### 1. Tóm Tắt Dự Án
**APT Magic** là một ứng dụng web serverless sử dụng trí tuệ nhân tạo (AI), cho phép người dùng tạo, cá nhân hóa và chia sẻ nội dung nghệ thuật như hình ảnh sinh bởi AI.  
Nền tảng này tích hợp với các mô hình nền tảng AI thông qua **Amazon Bedrock**, đồng thời mang lại trải nghiệm web liền mạch với **Next.js (SSR)** được lưu trữ trên **AWS Amplify**.  

Phiên bản **MVP (Minimum Viable Product)** tập trung vào việc tạo ảnh theo thời gian thực và chia sẻ nội dung.  
Trong khi đó, **phiên bản mở rộng (Future Design)** sẽ phát triển thêm các thành phần như **SageMaker Inference**, **Step Functions** và **AWS MLOps pipelines** để tự động hóa toàn bộ quy trình huấn luyện và triển khai mô hình.

APT Magic được xây dựng theo kiến trúc gốc AWS hiện đại, tối ưu chi phí, bảo mật cao, phù hợp cho nhóm người dùng nhỏ đến trung bình, và có khả năng mở rộng thành nền tảng AI cấp doanh nghiệp.

---

### 2. Vấn Đề & Giải Pháp

#### Vấn Đề Hiện Tại
Phần lớn các nền tảng tạo ảnh AI hiện nay có chi phí cao, phụ thuộc vào API bên thứ ba và thiếu khả năng tùy chỉnh cá nhân hóa.  
Các nhà phát triển và người sáng tạo nội dung thường gặp vấn đề về độ trễ, thiếu minh bạch trong việc quản lý mô hình, và hạn chế trong việc bảo mật dữ liệu người dùng.

#### Giải Pháp
**APT Magic** tận dụng kiến trúc **AWS serverless** để mang lại:
- Tạo ảnh AI theo thời gian thực thông qua mô hình **Amazon Bedrock Stability AI**.  
- Xác thực và quản lý nội dung người dùng bằng **Amazon Cognito** và **DynamoDB**.  
- Xử lý backend linh hoạt với **AWS Lambda** và **API Gateway**.  
- Cung cấp nội dung toàn cầu tốc độ cao với **CloudFront CDN** và bảo mật bằng **WAF**.  

Các bản mở rộng trong tương lai sẽ bao gồm **Step Functions orchestration**, **SQS/SNS decoupling**, **SageMaker Inference pipelines**, và CI/CD hiệu quả qua **CodeBuild**, **CodePipeline**, **CloudFormation** — biến APT Magic thành một nền tảng **MLOps** tự động hoàn chỉnh.

---

### 3. Kiến Trúc Giải Pháp

#### **Kiến Trúc MVP**
Phiên bản MVP sử dụng kiến trúc **hoàn toàn serverless**, tập trung vào khả năng mở rộng, dễ bảo trì và tối ưu chi phí.

**Các dịch vụ AWS chính:**
- **Route53 + CloudFront + WAF** — Bảo mật, phân phối và lưu trữ nội dung toàn cầu.  
- **Amplify (Next.js SSR)** — Lưu trữ frontend và lớp render phía server.  
- **API Gateway + Lambda Functions** — Xử lý backend (tạo ảnh, quản lý bài đăng, gói đăng ký...).  
- **Amazon Cognito** — Xác thực và phân quyền người dùng.  
- **Amazon S3 + DynamoDB** — Lưu trữ dữ liệu và hình ảnh.  
- **Amazon Bedrock** — Tích hợp mô hình nền tảng (Stability AI) để tạo ảnh.  
- **Secrets Manager, CloudWatch, CloudTrail** — Quản lý bảo mật, logging, giám sát.  

**Bảo mật**
- Sử dụng **PrivateLink** để kết nối an toàn giữa Lambda và các dịch vụ backend.  
- **WAF + IAM Policies** giúp lọc truy cập và quản lý quyền chi tiết.  

![Kiến trúc MVP của APT Magic](/images/aptMagic_mvp.png)

---

#### **Thiết Kế Tương Lai (Future Design)**
Ở giai đoạn tiếp theo, APT Magic sẽ phát triển thành một **nền tảng điều phối AI (AI Orchestration Platform)** với khả năng tự động hóa, phục hồi, và quản lý vòng đời mô hình.

**Dịch vụ bổ sung:**
- **AWS Step Functions** — Điều phối các quy trình bất đồng bộ như:
  - Quy trình nhiều bước cho tạo ảnh AI (xác thực prompt → suy luận → tải kết quả).  
  - Thanh toán → xử lý mô hình → thông báo người dùng.  
- **Amazon SQS** — Hàng đợi tin nhắn giữa các Lambda bất đồng bộ.  
- **Amazon SNS** — Thông báo thời gian thực đến người dùng / quản trị.  
- **Amazon ElastiCache (Redis)** — Giới hạn tốc độ và cache kết quả.  
- **Amazon SageMaker Inference** — Lưu trữ và triển khai mô hình tùy chỉnh.  
- **AWS CodePipeline + SageMaker Pipelines** — Tự động hóa huấn luyện, đánh giá, và triển khai mô hình.  
- **AWS PrivateLink + VPC Endpoints** — Đảm bảo luồng dữ liệu bảo mật giữa Lambda, S3, và SageMaker.  
- **AWS WAF & Shield Advanced** — Bảo vệ chống DDoS và lọc truy cập nâng cao.  

**CI/CD + MLOps**
- **CodePipeline + CodeBuild + CloudFormation** để tự động hóa hạ tầng và triển khai.  

![Kiến trúc tương lai của APT Magic](/images/aptMagic_future.png)

---

### 4. Triển Khai Kỹ Thuật

#### **Các Giai Đoạn Triển Khai**
**Giai đoạn 1 – MVP (Hiện tại / Hoàn tất):**
- Triển khai Amplify (Next.js SSR) + API Gateway + Lambda.  
- Tích hợp API Bedrock Stability AI.  
- Thiết lập CI/CD với CodePipeline + CloudFormation.  
- Kích hoạt xác thực người dùng (Cognito) và lưu trữ (S3 + DynamoDB).

**Giai đoạn 2 – Mở rộng trong tương lai:**
- Thêm Step Functions + SQS/SNS để xử lý workflow bất đồng bộ.  
- Dùng ElastiCache để cache và giới hạn tốc độ.  
- Tích hợp SageMaker Inference để lưu trữ mô hình tinh chỉnh.  
- Tự động hóa huấn luyện qua SageMaker Pipelines.  
- Mở rộng bảo mật với Shield Advanced + GuardDuty + PrivateLink.  
- Tích hợp GitLab Runner với CodeBuild cho CI/CD thống nhất.

---

### 5. Lộ Trình & Mốc Thời Gian

_(Phần này bạn có thể thêm timeline cụ thể nếu cần — ví dụ theo quý hoặc tháng.)_

---

### 6. Ước Tính Ngân Sách (AWS Pricing Estimate)

| Dịch vụ | Chi phí hàng tháng (ước tính) | Ghi chú |
|----------|-------------------------------|----------|
| Lambda + API Gateway | $0.50 | < 1 triệu lượt gọi |
| Amplify (Next.js SSR) | $0.35 | Lưu trữ và build website |
| S3 + DynamoDB | $0.20 | Lưu trữ hình ảnh & metadata |
| Bedrock Inference | $3.00 | Chi phí model (Stability AI) |
| ElastiCache (Future) | $1.00 | Cache giới hạn tốc độ |
| Step Functions + SQS/SNS | $0.60 | Quản lý workflow |
| SageMaker Inference (Future) | $5.00 | Chi phí endpoint mô hình |
| CloudWatch + WAF + Shield | $1.00 | Giám sát & bảo vệ |
| **Tổng (ước tính)** | **~$11.65/tháng** | Tăng theo lượng người dùng |

---

### 7. Đánh Giá Rủi Ro

| Rủi ro | Ảnh hưởng | Xác suất | Biện pháp giảm thiểu |
|--------|------------|----------|------------------------|
| Độ trễ khi suy luận mô hình | Trung bình | Cao | Dùng ElastiCache + Step Functions để xử lý bất đồng bộ |
| Chi phí tăng do gọi model nhiều | Cao | Trung bình | Quản lý hạn mức Bedrock, auto-scaling SageMaker |
| Lỗi cấu hình CI/CD | Trung bình | Thấp | Dùng CloudFormation rollback |
| Lỗ hổng bảo mật | Cao | Trung bình | Dùng WAF, GuardDuty, PrivateLink, phân quyền IAM tối thiểu |
| Phụ thuộc API bên thứ ba | Trung bình | Trung bình | Lưu trữ kết quả dự phòng trên S3 |

---

### 8. Kết Quả Mong Đợi

#### Kết Quả Kỹ Thuật:
- Hoàn thiện quy trình tạo ảnh AI serverless với CI/CD an toàn.  
- Kiến trúc mô-đun sẵn sàng mở rộng với MLOps.  
- Cải thiện tốc độ phản hồi và độ tin cậy qua caching & async.

#### Giá Trị Dài Hạn:
- Nền tảng sẵn sàng mở rộng thành **AI as a Service (AIaaS)**.  
- Hạ tầng **MLOps tự động** với khả năng huấn luyện lại mô hình.  
- Kiến trúc có thể tái sử dụng cho các sản phẩm AI khác trong tương lai.

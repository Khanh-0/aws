---
title: "Đề xuất"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# APT Magic

## Nền tảng AI serverless cho tạo ảnh cá nhân hoá và tương tác xã hội

### 1. Tóm tắt điều hành

**APT Magic** là một ứng dụng web serverless được trang bị AI, cho phép người dùng tạo, cá nhân hoá và chia sẻ nội dung nghệ thuật (ví dụ ảnh do AI tạo). Nền tảng tích hợp với các mô hình nền tảng qua **Amazon Bedrock** và cung cấp trải nghiệm web mượt mà bằng **Next.js (SSR)** được host trên **AWS Amplify**.

Phiên bản MVP tập trung vào tạo ảnh thời gian thực và chia sẻ, trong khi **Thiết kế tương lai** dự kiến mở rộng với **Bedrock AgentCore / SageMaker Inference**, **SQS/SNS**, **Secrets Manager & CloudTrail** và các pipeline **AWS MLOps** để điều phối mô hình và tự động hoá nâng cao.

APT Magic được phát triển theo kiến trúc AWS-native hiện đại, tối ưu chi phí và bảo mật cho nhóm người dùng nhỏ đến trung bình, với kế hoạch mở rộng lên điều phối AI cấp doanh nghiệp.

---

### 2. Vấn đề cần giải quyết

#### Vấn đề là gì?

Hầu hết nền tảng tạo ảnh AI hiện nay tốn kém, dựa vào API của bên thứ ba thiếu minh bạch và hạn chế khả năng cá nhân hoá. Nhà phát triển và creator thường gặp độ trễ cao, thiếu quản lý mô hình minh bạch và ít kiểm soát về bảo mật dữ liệu người dùng.

#### Giải pháp

APT Magic tận dụng **kiến trúc serverless trên AWS** để mang lại:

- Tạo ảnh AI thời gian thực thông qua các mô hình **Amazon Bedrock (Stability AI)**.
- Xác thực người dùng và quản lý nội dung an toàn với **Amazon Cognito** và **DynamoDB**.
- Xử lý API có thể mở rộng bằng **AWS Lambda** và **API Gateway**.
- Phân phối toàn cầu với độ trễ thấp qua **CloudFront CDN** và bảo vệ bằng **WAF**.

Các nâng cấp tương lai sẽ bổ sung **SQS/SNS** để để điều phối, **Bedrock AgentCore / SageMaker Inference** cho pipelines mô hình, và CI/CD hiệu quả chi phí thông qua **CloudFormation**, biến APT Magic thành nền tảng MLOps tự động hoàn toàn.

---

### 3. Kiến trúc giải pháp

#### **Kiến trúc MVP**

MVP là một **kiến trúc hoàn toàn serverless**, chú trọng khả năng mở rộng, dễ bảo trì và tối ưu chi phí.

**Dịch vụ AWS cốt lõi:**

- **Route53 + CloudFront + WAF** — Truy cập an toàn toàn cầu và caching.
- **Amplify (Next.js SSR)** — Host frontend và lớp server-side rendering.
- **API Gateway + Lambda Functions** — Quản lý logic backend (xử lý ảnh, subscription, API bài đăng).
- **Amazon Cognito** — Xác thực người dùng và quản lý quyền truy cập.
- **Amazon S3 + DynamoDB** — Lưu trữ ảnh và dữ liệu.
- **Amazon Bedrock** — Tích hợp mô hình nền tảng (Stability AI) cho tạo ảnh.
- **CloudWatch** — Ghi log và giám sát.

**Bảo mật**

- **WAF + IAM policies** để lọc lưu lượng và kiểm soát truy cập theo vai trò.

![APT Magic MVP Architecture](/images/2-Proposal/aptMagic_mvp.jpg)

---

#### **Thiết kế tương lai (Kiến trúc mở rộng)**

Trong giai đoạn tiếp theo, APT Magic sẽ phát triển thành một **nền tảng điều phối AI**, bổ sung các lớp tự động hoá, khả năng chịu lỗi và quản lý vòng đời mô hình.

**Dịch vụ mới sẽ thêm vào:**

- **Amazon SQS** — Hàng đợi tin nhắn tin cậy giữa các task Lambda bất đồng bộ.
- **Amazon SNS** — Thông báo sự kiện theo thời gian thực đến người dùng hoặc admin.
- **Amazon ElastiCache (Redis)** — Rate limiting và cache cho các yêu cầu inference thường xuyên.
- **Amazon Bedrock AgentCore** — Host các mô hình tùy chỉnh đã fine-tune và quản lý endpoint mô hình.

- **CI/CD**
  - **CloudFormation** cho triển khai hạ tầng tự động hoá.

![APT Magic Future Architecture](/images/2-Proposal/diagram_architecture.jpg)

---

### 4. Triển khai kỹ thuật

#### **Các giai đoạn triển khai**

**Giai đoạn 1 – Triển khai MVP (Đã hoàn thành / Hiện tại)**

- Triển khai Amplify (Next.js SSR) + API Gateway + Lambda.
- Tích hợp Bedrock Stability AI API.
- Thiết lập CI/CD qua GitLab CI/CD.
- Kích hoạt xác thực người dùng (Cognito) và lưu trữ (S3 + DynamoDB).
- Ghi log và giám sát qua CloudWatch.

**Giai đoạn 2 – Mở rộng Thiết kế Tương lai**

- Thêm SQS/SNS.
- Bổ sung ElastiCache cho throttling và caching.
- Tích hợp Bedrock Agent để nâng cao pipeline AI.
- Kết nối GitLab Runner với CodeBuild cho CI/CD thống nhất.

---

### 5. Timeline & Mốc quan trọng

| Giai đoạn                            | Mô tả                                                                                                      | Thời gian ước tính | Mốc triển khai                                                 |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------- | ------------------ | -------------------------------------------------------------- |
| **Tháng 1: Thiết lập & API cốt lõi** | Triển khai hạ tầng (IaC), Cognito, API Gateway, DynamoDB, và các Lambda cơ bản.                            | 4 Tuần             | Backend cốt lõi hoạt động, Auth/Quản lý người dùng hoàn thành. |
| **Tháng 2: Tích hợp AI**             | Tích hợp LLM trên Amazon Bedrock (Stability AI), Replicate API, hoàn thiện các chức năng xử lý ảnh.        | 4 Tuần             | Demo xử lý ảnh AI end-to-end thành công.                       |
| **Tháng 3: Front-end & CI/CD**       | Phát triển UI/UX (Amplify/Next.js), hoàn thiện pipeline CI/CD, cấu hình Giám sát/Bảo mật (CloudWatch/WAF). | 4 Tuần             | Nền tảng sẵn sàng cho thử nghiệm người dùng.                   |
| **Tháng 4: Tối ưu & Go-Live**        | Thực hiện test hiệu năng (Stress Test), tối ưu chi phí, và triển khai Production.                          | 4 Tuần             | **Go-Live** (Khởi chạy chính thức).                            |

---

### 6. Ước tính chi phí (Dự toán AWS)

#### Tổng chi phí

- **Hàng tháng:** $9.80
- **Trả trước:** $0.00
- **12 Tháng:** $117.60

---

#### Tổng quan dịch vụ

| Dịch vụ             | Vùng                     | Chi phí/tháng | Trả trước | Chi phí 12 tháng | Ghi chú                                                      |
| ------------------- | ------------------------ | ------------- | --------- | ---------------- | ------------------------------------------------------------ |
| Amazon Route 53     | Asia Pacific (Singapore) | $0.50         | $0.00     | $6.00            | 1 Hosted Zone, 1 domain, 1 linked VPC                        |
| Amazon CloudFront   | Asia Pacific (Singapore) | $0.00         | $0.00     | $0.00            | Không cấu hình đặc biệt                                      |
| AWS WAF             | Asia Pacific (Singapore) | $6.00         | $0.00     | $72.00           | 1 Web ACL; 1 rule per ACL                                    |
| AWS Amplify         | Asia Pacific (Singapore) | $0.00         | $0.00     | $0.00            | Build instance: Standard (8GB/4vCPU); request duration 500ms |
| AWS CloudFormation  | Asia Pacific (Singapore) | $0.00         | $0.00     | $0.00            | Không mở rộng; không thao tác                                |
| Amazon API Gateway  | Asia Pacific (Singapore) | $0.13         | $0.00     | $1.59            | 10k requests/month; WebSocket message 1KB; request size 30KB |
| AWS Lambda          | Asia Pacific (Singapore) | $1.67         | $0.00     | $20.04           | 1 million invokes; x86; 512MB ephemeral storage              |
| Amazon CloudWatch   | Asia Pacific (Singapore) | $0.85         | $0.00     | $10.22           | 1 metric; 0.5GB logs in; 0.5GB logs to S3                    |
| S3 Standard         | Asia Pacific (Singapore) | $0.23         | $0.00     | $2.76            | 10GB storage; 20k PUT; 40k GET                               |
| DynamoDB On-Demand  | Asia Pacific (Singapore) | $0.42         | $0.00     | $5.04            | 1GB storage; 1KB item; on-demand mode                        |
| **Tổng (Ước tính)** | —                        | **$9.80**     | **$0.00** | **$117.60**      | Dựa trên AWS Pricing Calculator                              |

---

#### Metadata

- **Tiền tệ:** USD
- **Locale:** en_US
- **Ngày tạo:** 12/9/2025
- **Share URL:** [AWS Calculator Link](https://calculator.aws/#/estimate?id=f8f785603d5dea16be2d60ad39e4733fc352a108)
- **Miễn trừ trách nhiệm:** Bảng ước tính chi phí chỉ mang tính tham khảo; chi phí thực tế có thể thay đổi theo mức sử dụng.

---

#### Giá mô hình AI

| Mô hình                    | Độ phân giải / Token      | Chất lượng | Giá/Yêu cầu (USD) | Ghi chú                                           |
| -------------------------- | ------------------------- | ---------- | ----------------- | ------------------------------------------------- |
| Titan Image Generator v2   | < 512×512                 | Standard   | 0.008             | Giá cố định cho 1 ảnh                             |
| Titan Image Generator v2   | < 512×512                 | Premium    | 0.01              | Giá cố định cho 1 ảnh                             |
| Titan Image Generator v2   | > 1024×1024               | Standard   | 0.01              | Giá cố định cho 1 ảnh                             |
| Titan Image Generator v2   | > 1024×1024               | Premium    | 0.012             | Giá cố định cho 1 ảnh                             |
| Stable Diffusion 3.5 Large | Any                       | N/A        | 0.08              | Giá cố định cho 1 ảnh                             |
| Claude (text + image)      | 40 input tokens + 1 image | N/A        | 0.00195           | Giá cho 1 request bao gồm text và 1 ảnh 1024×1024 |

#### Tuỳ chọn bổ sung

| Chế độ   | Augmentation | Giá (USD) |
| -------- | ------------ | --------- |
| text→img | no augment   | 0.08      |
| text→img | with augment | 0.08195   |
| img→img  | no augment   | 0.012     |
| img→img  | with augment | 0.094     |

---

### 7. Đánh giá rủi ro

| Rủi ro                      | Tác động   | Xác suất   | Biện pháp giảm thiểu                             |
| --------------------------- | ---------- | ---------- | ------------------------------------------------ |
| Độ trễ inference mô hình AI | Trung bình | Cao        | Dùng ElastiCache + SQS/SNS để xử lý bất đồng bộ  |
| Tăng chi phí do gọi mô hình | Cao        | Trung bình | Kiểm soát sử dụng Bedrock, autoscaling SageMaker |
| Lỗi cấu hình CI/CD          | Trung bình | Thấp       | Chính sách rollback CloudFormation               |
| Lỗ hổng bảo mật             | Cao        | Trung bình | WAF, GuardDuty, PrivateLink, IAM least privilege |
| Phụ thuộc API bên thứ ba    | Trung bình | Trung bình | Dự phòng: lưu kết quả inference vào S3           |

---

### 8. Kết quả kỳ vọng

#### Kết quả kỹ thuật:

- Hoàn thiện workflow tạo ảnh AI serverless với CI/CD bảo mật.
- Kiến trúc mô-đun cho phép tích hợp MLOps nhanh chóng.
- Cải thiện độ trễ và độ tin cậy bằng caching và workflow bất đồng bộ.

#### Giá trị dài hạn:

- Nền tảng cho mở rộng **AI as a Service (AIaaS)**.
- Khung **MLOps** sẵn sàng mở rộng với tự động huấn luyện lại.
- Hạ tầng đám mây có thể tái sử dụng cho sản phẩm AI trong tương lai.

---

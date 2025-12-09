---
title: "AWS Cloud Mastery Series #3: Trụ cột Bảo mật – AWS Well-Architected"
date: "2025-11-29T08:30:00+07:00"
weight: 9
chapter: false
pre: " <b> 4.9. </b> "
---

### Mục tiêu Sự kiện

- Giới thiệu cho người tham dự về **Trụ cột Bảo mật (Security Pillar)** trong AWS Well-Architected Framework
- Cung cấp kiến thức thực tiễn về danh tính, phát hiện, bảo vệ hạ tầng, bảo vệ dữ liệu và ứng phó sự cố
- Nâng cao nhận thức về các mối đe dọa bảo mật đám mây trong bối cảnh Việt Nam
- Khám phá các mô hình IAM hiện đại, phân tách mạng, chiến lược mã hóa và tự động hóa ứng phó sự cố
- Giúp người tham dự hiểu cách áp dụng Zero Trust và Defense-in-Depth trong môi trường AWS
- Xây dựng nền tảng kiến thức cho những ai muốn theo đuổi chứng chỉ AWS Security Specialty

### Đối tượng Tham dự

Workshop dành cho:

- Cloud engineers, security engineers, kiến trúc sư, DevOps engineers
- Quản trị viên CNTT và đội ngũ vận hành trên AWS
- Lập trình viên quan tâm đến các best practices về bảo mật
- Bất kỳ ai muốn củng cố nền tảng bảo mật đám mây

Không yêu cầu chuyên môn sâu, nhưng nên quen với AWS Console.

---

### Điểm Nhấn Quan Trọng

#### Nền tảng Bảo mật – Đặt vấn đề

Phiên mở đầu giới thiệu về:

- Vai trò của **Security Pillar** trong Well-Architected Framework
- Các nguyên tắc bảo mật cốt lõi: **Least Privilege**, **Zero Trust**, **Defense in Depth**
- **Mô hình Trách nhiệm Chia sẻ** và cách trách nhiệm thay đổi khi dùng dịch vụ managed
- Các mối đe dọa bảo mật đám mây phổ biến tại Việt Nam

Nền tảng này giúp người tham dự hiểu vì sao bảo mật phải được xây dựng ngay từ đầu, không phải bổ sung sau.

---

### Khám phá sâu 5 trụ cột bảo mật

Workshop được tổ chức xoay quanh **5 trụ cột bảo mật của AWS**, với hướng dẫn thực tế, demo và ví dụ thực tiễn:

- Kiến trúc IAM hiện đại với SSO, SCPs, permission boundaries
- Phát hiện & giám sát liên tục với CloudTrail, GuardDuty, Flow Logs, EventBridge
- Bảo vệ hạ tầng với phân tách VPC, WAF, Shield, Network Firewall
- Bảo vệ dữ liệu bằng mã hóa (KMS), quản lý vòng đời secrets và các guardrail
- Tự động hóa ứng phó sự cố bằng Lambda và Step Functions

---

### Agenda (Nội dung chương trình)

#### **Phiên Mở Đầu**

**8:30 – 8:50 AM** | Khởi động & Giới thiệu Nền tảng Bảo mật

- Vai trò của Security Pillar
- Nguyên tắc: Least Privilege, Zero Trust, Defense in Depth
- Mô hình Trách nhiệm Chia sẻ
- Các mối đe dọa đám mây hàng đầu tại Việt Nam

---

#### ⭐ **Trụ cột 1 — Identity & Access Management (IAM)**

**8:50 – 9:30 AM** | Kiến trúc IAM hiện đại

- IAM users, roles, policies — tránh dùng long-term credentials
- IAM Identity Center: SSO và permission sets
- SCPs & permission boundaries cho multi-account
- MFA, rotation, Access Analyzer
- **Mini Demo:** Kiểm tra IAM policies & mô phỏng quyền truy cập

---

#### ⭐ **Trụ cột 2 — Khả năng Phát hiện (Detection)**

**9:30 – 9:55 AM** | Phát hiện & Giám sát Liên tục

- CloudTrail (cấp tổ chức)
- GuardDuty & Security Hub
- Logging: VPC Flow Logs, ALB logs, S3 access logs
- Tự động hóa với EventBridge
- Detection-as-Code

**9:55 – 10:10 AM** | Nghỉ giải lao

---

#### ⭐ **Trụ cột 3 — Bảo vệ Hạ tầng (Infrastructure Protection)**

**10:10 – 10:40 AM** | Bảo mật mạng & workload

- Phân tách VPC: workloads công khai & riêng tư
- Security Groups vs NACLs
- WAF + Shield + Network Firewall
- Bảo vệ workload: EC2, ECS/EKS

---

#### ⭐ **Trụ cột 4 — Bảo vệ Dữ liệu (Data Protection)**

**10:40 – 11:10 AM** | Mã hóa, khóa & secrets

- KMS: key policy, grants, rotation
- Mã hóa at-rest & in-transit: S3, EBS, RDS, DynamoDB
- Secrets Manager & Parameter Store – rotation patterns
- Phân loại dữ liệu & guardrail truy cập

---

#### ⭐ **Trụ cột 5 — Ứng phó Sự cố (Incident Response)**

**11:10 – 11:40 AM** | Playbook & Tự động hóa IR

- Vòng đời ứng phó sự cố trên AWS
- Playbook cho:
  - IAM bị lộ credentials
  - S3 public exposure
  - EC2 có mã độc
- Isolation, snapshot, thu thập bằng chứng
- Tự động hóa bằng Lambda / Step Functions

---

#### **Phiên Tổng Kết**

**11:40 – 12:00 PM** | Tổng kết & Q&A

- Tóm tắt 5 trụ cột
- Các lỗi phổ biến tại doanh nghiệp Việt Nam
- Lộ trình học bảo mật (Security Specialty, SA Pro)

---

### Những Điều Rút Ra

#### Tư duy bảo mật

- Bảo mật phải liên tục và chủ động
- Least privilege, Zero Trust, Defense in Depth là nền tảng
- Hiểu mô hình trách nhiệm chia sẻ là điều bắt buộc

#### IAM & Detection

- SSO + permission sets giúp quản lý nhiều tài khoản dễ dàng
- SCPs cung cấp guardrail cấp tổ chức
- Giám sát liên tục với CloudTrail, GuardDuty, Flow Logs là bắt buộc

#### Bảo vệ Hạ tầng & Dữ liệu

- Phân tách VPC giúp giảm phạm vi ảnh hưởng khi xảy ra sự cố
- Mã hóa phải được áp dụng ở mọi nơi
- Quản lý vòng đời secrets giúp giảm nguy cơ bị xâm nhập

#### Ứng phó Sự cố

- Tự động hóa giúp rút ngắn thời gian xử lý sự cố
- Playbook giúp phản ứng nhanh và nhất quán
- Thu thập bằng chứng phải an toàn và bài bản

---

### Ứng dụng vào Công việc

- Áp dụng IAM Identity Center và SCPs trong môi trường multi-account
- Kích hoạt CloudTrail & GuardDuty cấp tổ chức
- Mã hóa & xoay vòng secrets cho mọi workload
- Dùng WAF, Shield để bảo vệ ứng dụng
- Xây dựng automation IR bằng Lambda hoặc Step Functions

---

### Trải nghiệm Sự kiện

Tham gia **AWS Well-Architected Security Pillar Workshop** giúp tôi hiểu sâu hơn về cách bảo mật cần được thiết kế, triển khai và tự động hóa trong AWS.

#### Học hỏi từ Chuyên gia AWS

- Nắm được các thực hành IAM quan trọng, phát hiện mối đe dọa và ứng phó sự cố
- Hiểu cách doanh nghiệp hiện đại triển khai multi-account security
- Nắm được các lỗi thường gặp trong môi trường thực tế

#### Hướng dẫn Thực hành

- Bài tập IAM giúp tôi hiểu rõ mô hình least-privilege
- Demo detection pipeline qua EventBridge & GuardDuty giúp tôi hình dung rõ hơn
- Playbook IR cho thấy tự động hóa quan trọng thế nào

#### Cộng đồng & Giao lưu

- Người tham dự chia sẻ nhiều tình huống thực tế
- Thảo luận về xu hướng bảo mật và các sai cấu hình phổ biến
- Workshop nhấn mạnh tầm quan trọng của việc học liên tục trong bảo mật đám mây

> Nhìn chung, workshop giúp tôi củng cố nền tảng bảo mật đám mây, tự tin hơn khi thiết kế hệ thống an toàn và phù hợp với AWS Well-Architected Framework.

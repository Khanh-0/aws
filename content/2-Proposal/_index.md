---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# APT Magic

## A Serverless AI Platform for Personalized Image Generation and Social Interaction

### 1. Executive Summary

**APT Magic** is a serverless AI-powered web application designed to enable users to generate, personalize, and share artistic content such as AI-generated images. The platform integrates with AI foundation models via **Amazon Bedrock** and provides a seamless web experience using **Next.js (SSR)** hosted on **AWS Amplify**.

The MVP version focuses on real-time image generation and sharing, while the **Future Design** aims to scale with **Bedrock agentCore/SageMaker Inference**, **SQS/SNS**, **Secret Manager & CloudTrail** and **AWS MLOps pipelines** for advanced model orchestration and automation.

APT Magic is currently developed as a modern, cost-efficient, and secure AWS-native architecture for small to medium user bases, with planned expansion into enterprise-grade AI orchestration.

---

### 2. Problem Statement

#### What’s the Problem?

Most AI image generation platforms are costly, rely on opaque third-party APIs, and offer limited personalization.  
Developers and creators often face high latency, lack of transparent model management, and limited control over user data security.

#### The Solution

APT Magic leverages **AWS serverless architecture** to deliver:

- Real-time AI image generation through **Amazon Bedrock Stability AI** models.
- Secure user authentication and content management using **Amazon Cognito** and **DynamoDB**.
- Scalable API handling via **AWS Lambda** and **API Gateway**.
- Low-latency global delivery with **CloudFront CDN** and **WAF protection**.

Future upgrades will include **SQS/SNS decoupling**, **Bedrock AgentCore/SageMaker Inference pipelines**, and cost-efficient CI/CD via **CloudFormation**. transforming APT Magic into a fully automated MLOps platform.

---

### 3. Solution Architecture

#### **MVP Architecture**

The MVP is a **fully serverless architecture**, focusing on scalability, maintainability, and cost-effectiveness.

**Core AWS Services:**

- **Route53 + CloudFront + WAF** — Secure global access and caching.
- **Amplify (Next.js SSR)** — Hosts the frontend and server-side rendering layer.
- **API Gateway + Lambda Functions** — Manage backend logic (image processing, subscription, post APIs).
- **Amazon Cognito** — User authentication and access control.
- **Amazon S3 + DynamoDB** — Data persistence and image storage.
- **Amazon Bedrock** — Integrates foundation model (Stability AI) for image generation.
- **CloudWatch** — Logging, and monitoring.

**Security**

- **WAF + IAM policies** for traffic filtering and role-based access control.

![APT Magic MVP Architecture](/images/2-Proposal/aptMagic_mvp.jpg)

---

#### **Future Design (Enhanced Architecture)**

In the next phase, APT Magic will evolve into an **AI orchestration platform**, introducing new layers for automation, resilience, and model lifecycle management.

**New Services to be Added:**

- **Amazon SQS** — For reliable message queuing between async Lambda tasks.
- **Amazon SNS** — For real-time event notifications to users or administrators.
- **Amazon ElastiCache (Redis)** — For rate limiting and caching of frequent inference requests.
- **Amazon Bedrock AgentCore** — For hosting custom fine-tuned models and managing model endpoints.

- **CI/CD**
  - **CloudFormation** for infrastructure deployment and automation.

![APT Magic Future Architecture](/images/2-Proposal/diagram_architecture.jpg)

---

### 4. Technical Implementation

#### **Implementation Phases**

**Phase 1 – MVP Deployment (Completed / Current)**

- Implement Amplify (Next.js SSR) + API Gateway + Lambda.
- Integrate Bedrock Stability AI API.
- Deploy CI/CD via Gitlab CI/CD.
- Enable user authentication (Cognito) and storage (S3 + DynamoDB).
- Log and Monitor via Cloudwatch

**Phase 2 – Future Design Expansion**

- Introduce SQS/SNS to decouple.
- Add ElastiCache for request throttling and caching.
- Integrate Bedrock Agent to enhance AI Pipelines
- Connect GitLab Runner with CodeBuild for unified CI/CD.

---

### 5. Timeline & Milestones

| Phase                               | Description                                                                                                          | Estimated Duration | Deployment Milestone                                      |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------ | --------------------------------------------------------- |
| **Month 1: Setup & Core API**       | Deploy infrastructure (IaC), Cognito, API Gateway, DynamoDB, and foundational Lambda functions.                      | 4 Weeks            | Core Backend operational, Auth/User Management completed. |
| **Month 2: AI Integration**         | Integrate Claude Haiku 3 LLM on Amazon Bedrock (Stability AI), Replicate API, complete _Image Processing_ functions. | 4 Weeks            | Successful end-to-end AI image processing demo.           |
| **Month 3: Front-end & CI/CD**      | Develop UI/UX (Amplify/Next.js), finalize CI/CD pipelines, and configure Monitoring/Security (CloudWatch/WAF).       | 4 Weeks            | Full platform ready for user testing.                     |
| **Month 4: Optimization & Go-Live** | Perform performance testing (Stress Test), cost optimization, and Production deployment.                             | 4 Weeks            | **Go-Live** (Official product launch).                    |

---

### 6. Cost Estimate (AWS Pricing Estimate)

#### Total Cost

- **Monthly:** $9.80
- **Upfront:** $0.00
- **12 Months:** $117.60

---

#### Service Overview

| Service              | Region                   | Monthly Cost | Upfront   | 12-Month Cost | Notes                                                        |
| -------------------- | ------------------------ | ------------ | --------- | ------------- | ------------------------------------------------------------ |
| Amazon Route 53      | Asia Pacific (Singapore) | $0.50        | $0.00     | $6.00         | 1 Hosted Zone, 1 domain, 1 linked VPC                        |
| Amazon CloudFront    | Asia Pacific (Singapore) | $0.00        | $0.00     | $0.00         | No specific configuration                                    |
| AWS WAF              | Asia Pacific (Singapore) | $6.00        | $0.00     | $72.00        | 1 Web ACL; 1 rule per ACL                                    |
| AWS Amplify          | Asia Pacific (Singapore) | $0.00        | $0.00     | $0.00         | Build instance: Standard (8GB/4vCPU); request duration 500ms |
| AWS CloudFormation   | Asia Pacific (Singapore) | $0.00        | $0.00     | $0.00         | No extensions; no operations                                 |
| Amazon API Gateway   | Asia Pacific (Singapore) | $0.13        | $0.00     | $1.59         | 10k requests/month; WebSocket message 1KB; request size 30KB |
| AWS Lambda           | Asia Pacific (Singapore) | $1.67        | $0.00     | $20.04        | 1 million invokes; x86; 512MB ephemeral storage              |
| Amazon CloudWatch    | Asia Pacific (Singapore) | $0.85        | $0.00     | $10.22        | 1 metric; 0.5GB logs in; 0.5GB logs to S3                    |
| S3 Standard          | Asia Pacific (Singapore) | $0.23        | $0.00     | $2.76         | 10GB storage; 20k PUT; 40k GET                               |
| DynamoDB On-Demand   | Asia Pacific (Singapore) | $0.42        | $0.00     | $5.04         | 1GB storage; 1KB item; on-demand mode                        |
| **Total (Estimate)** | —                        | **$9.80**    | **$0.00** | **$117.60**   | Based on AWS Pricing Calculator                              |

---

#### Metadata

- **Currency:** USD
- **Locale:** en_US
- **Created On:** 12/9/2025
- **Share URL:** [AWS Calculator Link](https://calculator.aws/#/estimate?id=f8f785603d5dea16be2d60ad39e4733fc352a108)
- **Legal Disclaimer:** AWS Pricing Calculator provides estimates only; actual costs may vary based on usage.

---

#### AI Model Pricing

| Model                      | Resolution / Token Usage  | Quality  | Price per Request (USD) | Notes                                                    |
| -------------------------- | ------------------------- | -------- | ----------------------- | -------------------------------------------------------- |
| Titan Image Generator v2   | < 512×512                 | Standard | 0.008                   | Fixed price per 1 image                                  |
| Titan Image Generator v2   | < 512×512                 | Premium  | 0.01                    | Fixed price per 1 image                                  |
| Titan Image Generator v2   | > 1024×1024               | Standard | 0.01                    | Fixed price per 1 image                                  |
| Titan Image Generator v2   | > 1024×1024               | Premium  | 0.012                   | Fixed price per 1 image                                  |
| Stable Diffusion 3.5 Large | Any                       | N/A      | 0.08                    | Fixed price per 1 image                                  |
| Claude (text + image)      | 40 input tokens + 1 image | N/A      | 0.00195                 | Price for 1 request including text and 1 image 1024×1024 |

#### Additional Options

| Mode     | Augmentation | Price (USD) |
| -------- | ------------ | ----------- |
| text→img | no augment   | 0.08        |
| text→img | with augment | 0.08195     |
| img→img  | no augment   | 0.012       |
| img→img  | with augment | 0.094       |

---

### 7. Risk Assessment

| Risk                           | Impact | Probability | Mitigation                                       |
| ------------------------------ | ------ | ----------- | ------------------------------------------------ |
| AI model inference latency     | Medium | High        | Use ElastiCache + SQS/SNS for async handling     |
| Cost increase from model calls | High   | Medium      | Bedrock usage control, SageMaker autoscaling     |
| CI/CD misconfigurations        | Medium | Low         | CloudFormation rollback policies                 |
| Security vulnerabilities       | High   | Medium      | WAF, GuardDuty, PrivateLink, IAM least privilege |
| Third-party API dependency     | Medium | Medium      | Bedrock fallback to S3-stored inference results  |

---

### 8. Expected Outcomes

#### Technical Outcomes:

- Complete serverless AI image generation workflow with secure CI/CD.
- Modular orchestration enabling rapid MLOps integration.
- Improved latency and reliability via caching and async workflows.

#### Long-Term Value:

- A foundation for **AI as a Service (AIaaS)** platform expansion.
- Ready-to-scale **MLOps framework** with automated retraining.
- Reusable cloud infrastructure for future AI products.

---
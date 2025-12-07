---
title : "Cấu hình & Deploy AgentCore"

weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

## Bắt đầu với AgentCore Configure

Đầu tiên, bạn cần đẩy code từ máy local lên AWS AgentCore bằng lệnh:

```bash
agentcore configure -e ./{ten_file_python.py}
````
![agentsetup1](/aws/static/images/5-Workshop/5.4-S3-onprem/agentsetup1.png)

![agentsetup2](/aws/static/images/5-Workshop/5.4-S3-onprem/agentsetup2.png)

##### 1. Agent Name

Nhập tên cho Agent của bạn.


##### 2. Configuration File

Nhấn **Enter** để dùng file cấu hình mặc định `pyproject.toml`.


##### 3. Deployment Configuration

Chọn **2 – Deploy bằng Docker** để AgentCore tự tạo Docker image và quản lý dễ dàng hơn.



##### 4. Execution Role

Để mặc định và cho AWS tự tạo IAM Role.



##### 5. ECR Repository

Nhấn **Enter** để AWS tự tạo nơi lưu Docker image.



##### 6. Authorization Configuration

Chọn **No** cho OAuth.
Agent chỉ cho phép truy cập qua AWS IAM Access Key & Secret Key.



##### 7. Request Header Allowlist

Nhấn **Enter** để dùng cấu hình mặc định.



####  Kết quả

Hoàn tất bước này là bạn đã đẩy code lên AgentCore thành công.



Khởi chạy Agent

Dùng lệnh sau để chạy Agent với API Key (dùng GROQ):


agentcore launch --env GROQ_API_KEY=your_api_key_here


Khi terminal báo trạng thái Running, Agent của bạn đã hoạt động thành công.

![goodrun](/aws/static/images/5-Workshop/5.4-S3-onprem/agentsetupdone.png)



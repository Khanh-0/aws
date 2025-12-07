---
title : "Agent core memory"
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Cấu hình Memory trong AgentCore

Để tạo bộ nhớ cho AgentCore, thực hiện theo các bước dưới đây.

## 1. Tạo Memory trong Bedrock

1. Vào **Bedrock** → chọn **AgentCore**.  
2. Chuyển sang tab **Memory**.  
3. Nhấn **Create memory**.


Trong giao diện tạo Memory, bạn sẽ thấy các phần sau:


###  Memory name  
Đặt tên bộ nhớ mà AgentCore sẽ sử dụng.

###  Short-term memory (raw event) expiration  
Số ngày hệ thống lưu lại lịch sử hội thoại chi tiết.  
Demo này có thể để mặc định **90 ngày**.

![memory](/images/5-Workshop/5.3-S3-vpc/memory.png)


## 2. Các loại Memory trong AgentCore
![memory](/images/5-Workshop/5.3-S3-vpc/typememory.png)

### 1. Summarization – Tóm tắt hội thoại  
**Chức năng:** Tóm tắt nội dung hội thoại sau khi kết thúc hoặc theo chu kỳ.  
**Tác dụng:** Giữ lại ngữ cảnh dài hạn mà không chiếm nhiều bộ nhớ.

**Ví dụ:**  
Bạn chat 100 câu về lỗi AWS CLI → lần sau Agent nhớ:  
> “Người dùng đang gặp lỗi kết nối AWS CLI.”

### 2. Semantic Memory – Bộ nhớ ngữ nghĩa  
**Chức năng:** Lưu trữ các facts/kiến thức quan trọng, độc lập với ngữ cảnh.  
**Tác dụng:** Dùng để trả lời các câu hỏi liên quan đến thông tin đã nói trước đó.

**Ví dụ:**  
> “Dự án A dùng Python 3.9.”  
Lần sau hỏi lại → Agent trả lời ngay **Python 3.9**.

### 3. User Preferences – Sở thích người dùng  
**Chức năng:** Tự nhận biết thói quen và phong cách của người dùng.  
**Tác dụng:** Cá nhân hóa cách phản hồi.

**Ví dụ:**  
Bạn hay yêu cầu:  
> “Hãy trả lời ngắn gọn.”  
Agent sẽ tự động trả lời đúng ý mỗi lần.

### 4. Episodes – Bộ nhớ tình huống  
**Chức năng:** Lưu chuỗi sự kiện, phân tích thành công/thất bại qua Reflections.  
**Tác dụng:** Giúp Agent học từ kinh nghiệm.

**Ví dụ:**  
Lần trước đặt vé bị lỗi vì thiếu ngày tháng → Agent nhớ.  
Lần sau nó sẽ hỏi ngày tháng trước.



## 3. Loại Memory dùng trong Demo

Trong phần demo, chỉ cần dùng loại:

### Summarization  
Chọn Summarization rồi nhấn **Create** để hoàn tất.



## 4. Cập nhật Memory ID trong Python

Sau khi tạo Memory, bạn sẽ nhận được **Memory ID**.

Thêm vào file Python như sau:

```python
# AgentCore Memory Configuration
REGION = "ap-southeast-1"
MEMORY_ID = "memory_j98zj-4LFDxqB2o1"
GROQ_API_KEY = os.getenv("GROQ_API_KEY")
```
lưu ý thay đổi **Id Memory** và **Region** theo đúng cái bạn đã tạo
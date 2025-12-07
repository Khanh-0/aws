---
title : "Gọi AgentCore"

weight : 1
chapter : false
pre : " <b> 5.4.2 </b> "
---

##  Demo đơn giản với AgentCore

###  1. Gửi câu hỏi đầu tiên

Dùng lệnh:

``` bash
agentcore invoke "{'prompt': 'Tell me about roaming activations'}"
```
![test1](/images/5-Workshop/5.4-S3-onprem/test1.png)

Agent sẽ trả lời dựa trên dữ liệu bạn đã triển khai (database + logic
trong code).



###  2. Kiểm tra khả năng ghi nhớ giữa các lần gọi (session)

Sau khi hỏi lần đầu, bạn tiếp tục hỏi một câu có liên quan --- ví dụ
``` bash
agentcore invoke "{'prompt': 'Activate it for Vietnam'}"
```
![test2](/images/5-Workshop/5.4-S3-onprem/test2.png)
``` bash
agentcore invoke "{'prompt': 'which country was i referring to'}"
```
![test3](/images/5-Workshop/5.4-S3-onprem/test3.png)
Nếu Agent phản hồi đúng ý và nhớ thông tin trước đó → chứng tỏ **Memory
hoạt động** và AgentCore đang lưu ngữ cảnh giữa các lần gọi.


AgentCore hoạt động đúng như mong đợi trong demo này.
---
title: "Gọi Groq API"
weight : 3
chapter : false
pre : " <b> 5.3.2 </b> "
---

## Mục tiêu

Sử dụng thư viện Groq (ở đây là `ChatGroq` / `init_chat_model` với `model_provider="groq"`) để gọi model OpenAI (Groq-hosted).

## Cấu hình trong Code

Trong code demo:

### Lấy API Key từ Environment

```python
GROQ_API_KEY = os.getenv("GROQ_API_KEY")
```

Biến `GROQ_API_KEY` lấy API key từ environment variable.

### Khởi tạo Model

```python
llm = init_chat_model(
    model="openai/gpt-oss-20b", 
    model_provider="groq",
    api_key=GROQ_API_KEY
)
```

### Tích hợp vào Agent

Agent gọi LLM thông qua `create_agent(...)` với tham số `model=llm`:

```python
agent = create_agent(
    model=llm,
    tools=tools,
    checkpointer=checkpointer,
    store=store,
    middleware=[MemoryMiddleware()],
    system_prompt=system_prompt,
)
```

## Luồng xử lý

```
Agent → Groq API → Model Inference → Response
```


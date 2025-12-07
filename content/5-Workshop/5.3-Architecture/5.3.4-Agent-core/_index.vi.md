---
title: "Code AgentCore"
weight : 3
chapter : false
pre : " <b> 5.3.4 </b> "
---

## Mục tiêu

Giải thích hàm xử lý trong agent, cách agent gọi Groq, và cách gắn memory ID vào agent để duy trì context lâu dài.

## Entrypoint AgentCore

### Đăng ký Entrypoint

Hàm entrypoint được đăng ký bằng decorator `@app.entrypoint`:

```python
@app.entrypoint
def agent_invocation(payload, context):
    """
    Main entry point for agent execution
    
    Args:
        payload: Input data containing prompt, actor_id, thread_id
        context: Runtime metadata
    """
    # ... xử lý logic
```

### Input Parameters

- **`payload`**: Chứa input từ client
  - `prompt`: Câu hỏi người dùng
  - `actor_id`: ID người dùng (optional)
  - `thread_id`: ID phiên chat (optional)
- **`context`**: Metadata runtime từ AgentCore

## Cấu hình Memory

### Lấy Actor ID và Thread ID

```python
actor_id = payload.get("actor_id", "default-user")
thread_id = payload.get(
    "thread_id", 
    payload.get("session_id", "default-session")
)

config = {
    "configurable": {
        "thread_id": thread_id,
        "actor_id": actor_id
    }
}
```

### Memory ID

`MEMORY_ID` là identifier duy nhất cho memory storage:

```python
MEMORY_ID = "memory_j98zj-4LFDxqB2o1"

# Khởi tạo memory components
checkpointer = AgentCoreMemorySaver(memory_id=MEMORY_ID)
store = AgentCoreMemoryStore(memory_id=MEMORY_ID)
```

**Vai trò của Memory ID**:
- Trỏ tới namespace lưu trữ memory (S3/DynamoDB/Bedrock)
- Đảm bảo nhất quán giữa các instance
- Cho phép chia sẻ memory pool nếu cần

### Tích hợp Memory vào Agent

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

## Memory Middleware

### Cấu trúc Middleware

`MemoryMiddleware` có hai hook chính:

#### 1. Pre-Model Hook

Chạy **trước** khi gọi model:

```python
def pre_model_hook(self, state, config):
    """
    Xử lý trước khi gọi LLM
    - Lưu user message vào memory
    - Load preferences và past memories
    - Enrich context
    """
    actor_id = config["configurable"]["actor_id"]
    thread_id = config["configurable"]["thread_id"]
    
    # Lấy messages hiện tại
    messages = state.get("messages", [])
    
    # Lưu user message
    for msg in messages:
        if msg.type == "human":
            namespace = (actor_id, thread_id)
            store.put(
                namespace, 
                str(uuid.uuid4()), 
                {"message": msg}
            )
    
    # Load memories và preferences
    memories = store.search(namespace, query=msg.content)
    # Append vào context...
```

#### 2. Post-Model Hook

Chạy **sau** khi model trả về:

```python
def post_model_hook(self, state, config):
    """
    Xử lý sau khi LLM response
    - Lưu AI message vào long-term memory
    - Update statistics
    """
    messages = state.get("messages", [])
    
    # Lưu AI response
    for msg in messages:
        if msg.type == "ai":
            namespace = (actor_id, thread_id)
            store.put(
                namespace,
                str(uuid.uuid4()),
                {"message": msg}
            )
```

### Mục đích Memory Middleware

- Duy trì **lịch sử hội thoại dài hạn**
- **Personalization**: Nhớ preferences người dùng
- **Context enrichment**: Thêm thông tin relevant từ quá khứ
- **Analytics**: Track conversation patterns

## Agent Execution Flow

### 1. Invoke Agent

```python
query = payload.get("prompt", "")

result = agent.invoke(
    {"messages": [("human", query)]},
    config=config
)
```

### 2. Agent Processing Pipeline

```
1. Pre-Model Hook
   ├── Lưu user message
   ├── Load memories
   └── Enrich context
   
2. Tool Decision
   ├── search_faq()
   ├── search_detailed_faq()
   └── reformulate_query()
   
3. Call LLM (Groq)
   ├── Send prompt + context
   └── Receive response
   
4. Post-Model Hook
   ├── Lưu AI response
   └── Update memory
```

### 3. Trích xuất Kết quả

```python
messages = result.get("messages", [])
answer = messages[-1].content if messages else "No response generated"

return {
    "result": answer,
    "actor_id": actor_id,
    "thread_id": thread_id
}
```

## Tool Functions

Agent có thể sử dụng các tools để tăng độ chính xác:

### Định nghĩa Tool

```python
@tool
def search_faq(query: str) -> str:
    """
    Search FAQ database for quick answers
    
    Args:
        query: User question
        
    Returns:
        Relevant FAQ content
    """
    results = faq_store.similarity_search(query, k=3)
    return "\n".join([doc.page_content for doc in results])
```

### Các Tool Khác

```python
@tool
def search_detailed_faq(query: str) -> str:
    """Tìm kiếm chi tiết với k=5"""
    results = faq_store.similarity_search(query, k=5)
    return format_results(results)

@tool
def reformulate_query(original_query: str) -> str:
    """Cải thiện câu query để tìm kiếm tốt hơn"""
    # Logic reformulation...
    return improved_query
```

### Vai trò Tools

- **Tách Retrieval khỏi Generation** (R vs G trong RAG)
- **Cải thiện độ chính xác**: Agent quyết định khi nào dùng tool
- **Flexible workflow**: Có thể chain nhiều tools



## Example Payload & Response

### Input Payload

```json
{
  "prompt": "Làm thế nào để reset mật khẩu?",
  "actor_id": "user_12345",
  "thread_id": "session_abc"
}
```

### Output Response

```json
{
  "result": "Để reset mật khẩu, bạn có thể:\n1. Vào Settings > Security\n2. Chọn 'Reset Password'\n3. Nhập email để nhận link reset",
  "actor_id": "user_12345",
  "thread_id": "session_abc"
}
```

## Best Practices

### Memory Management

- **Consistent Memory ID**: Dùng cùng `MEMORY_ID` cho cùng một service
- **Namespace isolation**: Sử dụng `(actor_id, thread_id)` để tách biệt conversations
- **Cleanup policy**: Định kỳ xóa old memories để tối ưu storage

### Error Handling

```python
@app.entrypoint
def agent_invocation(payload, context):
    try:
        # ... main logic
        result = agent.invoke(...)
        return {"result": answer, ...}
    
    except Exception as e:
        logger.error(f"Agent error: {e}")
        return {
            "error": str(e),
            "actor_id": actor_id,
            "thread_id": thread_id
        }
```

### Performance

- **Async tools** nếu có I/O operations
- **Cache embeddings** để tránh recompute
- **Batch processing** cho multiple queries

## Checklist Triển khai

- [ ] Định nghĩa `MEMORY_ID` duy nhất
- [ ] Implement `AgentCoreMemorySaver` và `AgentCoreMemoryStore`
- [ ] Tạo `MemoryMiddleware` với pre/post hooks
- [ ] Định nghĩa tools (`@tool` decorator)
- [ ] Implement entrypoint function
- [ ] Test với payload mẫu
- [ ] Verify memory persistence
- [ ] Monitor tool usage và LLM calls


Kết quả: Một AI agent thông minh, có khả năng nhớ context và tự động chọn công cụ phù hợp.


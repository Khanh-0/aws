---
title: "Chunking & Embedding"
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

## Lý do Chunking

Tài liệu/FAQ thường dài; để tính embedding hiệu quả và tối ưu truy vấn tương đồng, cần chia văn bản lớn thành các đoạn nhỏ (chunk).

### Lợi ích

- **Giảm loss of context** khi embed
- **Retrieval chính xác hơn** bằng vector similarity
- **Tối ưu hiệu suất** khi tìm kiếm
- **Phù hợp với giới hạn token** của model embedding

## Chiến lược Chunking

Trong code sử dụng `RecursiveCharacterTextSplitter`:

```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, 
    chunk_overlap=0
)
chunks = splitter.split_documents(docs)
```

### Tham số Chunking

| Tham số | Giá trị | Ý nghĩa |
|---------|---------|---------|
| `chunk_size` | 500 | Kích thước mỗi chunk (characters) |
| `chunk_overlap` | 0 | Không có phần chồng lấn giữa các chunk |

### Gợi ý Tối ưu

- **Kích thước chunk hợp lý**: 500–1000 tokens (tùy model embedding)
- **Chunk overlap**: Nếu cần context liên tục, set overlap 50-100 characters
- **Trade-off**: Chunk nhỏ → chính xác cao nhưng nhiều vectors; Chunk lớn → ít vectors nhưng có thể mất ngữ cảnh

## Tạo Embedding và Vector Store

### Khởi tạo Embedding Model

```python
emb = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

**Model được chọn**: `all-MiniLM-L6-v2`
- Lightweight và nhanh
- Phù hợp cho tiếng Anh
- Kích thước embedding: 384 dimensions

### Tạo FAISS Vector Store

```python
faq_store = FAISS.from_documents(chunks, emb)
```

FAISS (Facebook AI Similarity Search) cung cấp:
- Tìm kiếm vector nhanh chóng
- Hiệu quả với datasets lớn
- Hỗ trợ nhiều thuật toán index

### Truy vấn Vector Store

```python
results = faq_store.similarity_search(query, k=3)
```

Tham số:
- `query`: Câu hỏi người dùng
- `k=3`: Trả về top 3 chunks có độ tương đồng cao nhất


## Ghi chú Quan trọng

### Cập nhật Dữ liệu

- Nếu dữ liệu thay đổi (add/update documents), cần:
  - Re-embed toàn bộ hoặc
  - Incremental update vector store

### Chọn Embedding Model

Trade-off cần cân nhắc:

| Tiêu chí | Lightweight Model | Heavy Model |
|----------|-------------------|-------------|
| Tốc độ |  Nhanh |  Chậm |
| Độ chính xác |  Tốt |  Rất tốt |
| Chi phí |  Thấp |  Cao |
| Use case | FAQ, chatbot | Research, legal |

### Model tiếng Việt

Nếu cần hỗ trợ tiếng Việt tốt hơn:
- `keepitreal/vietnamese-sbert`
- `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

## Code Hoàn chỉnh

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS

# Load documents
docs = load_faq_csv()

# Chunking
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, 
    chunk_overlap=0
)
chunks = splitter.split_documents(docs)

# Embedding
emb = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)

# Vector Store
faq_store = FAISS.from_documents(chunks, emb)

# Query
query = "Làm thế nào để đổi mật khẩu?"
results = faq_store.similarity_search(query, k=3)
```

## Checklist Triển khai

- [ ] Chuẩn bị documents (CSV, JSON, text files)
- [ ] Chọn chunk_size phù hợp (test với 500, 750, 1000)
- [ ] Chọn embedding model (tiếng Anh hoặc đa ngôn ngữ)
- [ ] Tạo và lưu FAISS index
- [ ] Test retrieval với các query mẫu
- [ ] Monitor và điều chỉnh k value


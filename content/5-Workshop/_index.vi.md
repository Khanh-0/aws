---
title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng RAG Agent với Groq API và AgentCore Memory

#### Tổng quan

Trong workshop này, chúng ta sẽ xây dựng một **RAG (Retrieval-Augmented Generation) Agent** hoàn chỉnh với khả năng:

- **Gọi Groq API** để sử dụng LLM models với hiệu năng cao
- **Chunking & Embedding** documents để tối ưu vector search
- **AgentCore Memory** để duy trì context lâu dài qua các phiên chat
- **Tool Integration** để agent có thể tự động search FAQ và reformulate queries

**AgentCore** cung cấp framework để xây dựng AI agents với memory persistence, middleware hooks, và tool orchestration - cho phép agent "nhớ" lịch sử hội thoại và personalize responses.


#### Nội dung

1. [Tổng quan về Workshop](5.1-workshop-overview/)
2. [Chuẩn bị](5.2-prerequiste/)
3. [Kiến trúc](5.3-architecture/)
   - [5.3.1. Gọi Groq API](5.3-architecture/5.3.1-goi-groq-api/)
   - [5.3.2. Chunking & Embedding](5.3-architecture/5.3.2-chunking-embedding/)
   - [5.3.3. Code Handler AgentCore](5.3-architecture/5.3.3-code-handler-agentcore/)
4. [Chạy Agent Core](5.4-agent-core-run/)

#### Tech Stack

| Component | Technology |
|-----------|-----------|
| **LLM Provider** | Groq API (OpenAI models) |
| **Embedding Model** | HuggingFace (all-MiniLM-L6-v2) |
| **Vector Store** | FAISS |
| **Agent Framework** | LangChain + AgentCore |
| **Memory Backend** | AgentCore Memory Store |
| **Text Splitting** | RecursiveCharacterTextSplitter |

#### Điều kiện tiên quyết

- Python 3.8+
- Groq API Key
- Kiến thức cơ bản về RAG và LLM
- Hiểu biết về vector embeddings

#### Kết quả mong đợi

Sau workshop, bạn sẽ có:

Agent có khả năng trả lời FAQ dựa trên vector search  
Memory system để nhớ preferences và context người dùng  
Tool orchestration để agent tự quyết định khi nào dùng tool  
Production-ready code với error handling và logging  

---

**Bắt đầu**: [5.1. Tổng quan về Workshop](5.1-workshop-overview/)
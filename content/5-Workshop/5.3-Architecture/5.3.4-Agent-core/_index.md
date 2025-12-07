---

title: "5.3.2. Chunking & Embedding"
weight: 3
chapter: false
pre: " <b> 5.3.3 </b> "
----
## Why Chunking Matters

Documents and FAQs are often long; to generate embeddings effectively and optimize similarity search, large text needs to be split into smaller segments (chunks).

### Benefits

* **Reduce loss of context** during embedding
* **More accurate retrieval** using vector similarity
* **Better performance** during search
* **Avoid token limit issues** of embedding models

## Chunking Strategy

The code uses `RecursiveCharacterTextSplitter`:

```python
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500, 
    chunk_overlap=0
)
chunks = splitter.split_documents(docs)
```

### Chunking Parameters

| Parameter       | Value | Meaning                         |
| --------------- | ----- | ------------------------------- |
| `chunk_size`    | 500   | Size of each chunk (characters) |
| `chunk_overlap` | 0     | No overlap between chunks       |

### Optimization Suggestions

* **Recommended chunk size**: 500–1000 tokens (depends on the embedding model)
* **Chunk overlap**: If you need continuous context, set overlap to 50–100 characters
* **Trade-off**:

  * Small chunks → higher accuracy but more vectors
  * Large chunks → fewer vectors but potential context loss

## Creating Embeddings & Vector Store

### Initialize the Embedding Model

```python
emb = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

**Chosen model**: `all-MiniLM-L6-v2`

* Lightweight and fast
* Good for English
* Embedding size: 384 dimensions

### Create FAISS Vector Store

```python
faq_store = FAISS.from_documents(chunks, emb)
```

FAISS (Facebook AI Similarity Search) provides:

* Fast vector search
* Efficient for large datasets
* Supports multiple index algorithms

### Querying the Vector Store

```python
results = faq_store.similarity_search(query, k=3)
```

Parameters:

* `query`: User question
* `k=3`: Returns top 3 most similar chunks

## Important Notes

### Data Updates

If your data changes (add/update documents), you need to:

* Re-embed all documents, or
* Incrementally update the vector store

### Choosing an Embedding Model

| Criteria  | Lightweight Model | Heavy Model                  |
| --------- | ----------------- | ---------------------------- |
| Speed     | Fast              | Slow                         |
| Accuracy  | Good              | Excellent                    |
| Cost      | Low               | High                         |
| Use cases | FAQ, chatbot      | Research, legal, complex RAG |

### Vietnamese Models

If you need better Vietnamese support:

* `keepitreal/vietnamese-sbert`
* `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

## Full Code Example

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
query = "How do I change my password?"
results = faq_store.similarity_search(query, k=3)
```

## Implementation Checklist

* [ ] Prepare documents (CSV, JSON, text files)
* [ ] Choose chunk_size (test 500, 750, 1000)
* [ ] Select embedding model (English or multilingual)
* [ ] Build and save FAISS index
* [ ] Test retrieval with sample queries
* [ ] Monitor and adjust k value


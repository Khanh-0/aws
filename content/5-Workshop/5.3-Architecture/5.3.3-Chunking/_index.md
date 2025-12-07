---

title: "Chunking & Embedding"
weight: 3
chapter: false
pre: " <b> 5.3.3 </b> "
----

## Why Chunking Is Needed

Documents or FAQs are often long; to compute embeddings effectively and optimize similarity search, large text must be split into smaller segments (chunks).

### Benefits

* **Reduces context loss** during embedding
* **More accurate retrieval** with vector similarity
* **Better performance** for search
* **Compatible with token limits** of embedding models

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

### Optimization Tips

* **Recommended chunk size**: 500–1000 tokens (depends on embedding model)
* **Chunk overlap**: Use 50–100 characters if continuous context is needed
* **Trade-off**:

  * Smaller chunks → higher accuracy but more vectors
  * Larger chunks → fewer vectors but may lose context

## Creating Embeddings and Vector Store

### Initialize Embedding Model

```python
emb = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

**Selected model**: `all-MiniLM-L6-v2`

* Lightweight and fast
* Good for English
* Embedding dimension: 384

### Create FAISS Vector Store

```python
faq_store = FAISS.from_documents(chunks, emb)
```

FAISS (Facebook AI Similarity Search) provides:

* High-speed vector search
* Efficient handling of large datasets
* Multiple index algorithms

### Query the Vector Store

```python
results = faq_store.similarity_search(query, k=3)
```

Parameters:

* `query`: User question
* `k=3`: Returns top 3 most relevant chunks

## Important Notes

### Updating Data

If documents change (add/update):

* Re-embed all data, or
* Apply incremental updates to the vector store

### Choosing an Embedding Model

Trade-off comparison:

| Criteria  | Lightweight Model | Heavy Model     |
| --------- | ----------------- | --------------- |
| Speed     | Fast              | Slow            |
| Accuracy  | Good              | Very good       |
| Cost      | Low               | High            |
| Use cases | FAQ, chatbot      | Research, legal |

### Vietnamese Models

For better Vietnamese performance:

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

## Deployment Checklist

* [ ] Prepare documents (CSV, JSON, text files)
* [ ] Select appropriate `chunk_size` (test 500, 750, 1000)
* [ ] Choose embedding model (English or multilingual)
* [ ] Build and save FAISS index
* [ ] Test retrieval with sample queries
* [ ] Monitor and tune the `k` value for similarity search

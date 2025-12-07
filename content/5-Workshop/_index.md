---

title: "Workshop"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Building a RAG Agent with Groq API and AgentCore Memory

#### Overview

In this workshop, we will build a complete **RAG (Retrieval-Augmented Generation) Agent** with the following capabilities:

* **Calling the Groq API** to use high-performance LLM models
* **Chunking & Embedding** documents for optimized vector search
* **AgentCore Memory** to maintain long-term context across chat sessions
* **Tool Integration** so the agent can automatically search FAQs and reformulate queries

**AgentCore** provides a framework for building AI agents with memory persistence, middleware hooks, and tool orchestration — enabling the agent to “remember” conversation history and personalize responses.

#### Contents

1. [Workshop Overview](5.1-workshop-overview/)
2. [Prerequisites](5.2-prerequiste/)
3. [Architecture](5.3-architecture/)

   * [5.3.1. Calling Groq API](5.3-architecture/5.3.1-goi-groq-api/)
   * [5.3.2. Chunking & Embedding](5.3-architecture/5.3.2-chunking-embedding/)
   * [5.3.3. AgentCore Code Handler](5.3-architecture/5.3.3-code-handler-agentcore/)
4. [Running AgentCore](5.4-agent-core-run/)
5. [Resource Cleanup](5.6-cleanup/)

#### Tech Stack

| Component           | Technology                     |
| ------------------- | ------------------------------ |
| **LLM Provider**    | Groq API (OpenAI models)       |
| **Embedding Model** | HuggingFace (all-MiniLM-L6-v2) |
| **Vector Store**    | FAISS                          |
| **Agent Framework** | LangChain + AgentCore          |
| **Memory Backend**  | AgentCore Memory Store         |
| **Text Splitting**  | RecursiveCharacterTextSplitter |

#### Prerequisites

* Python 3.8+
* Groq API Key
* Basic understanding of RAG and LLMs
* Familiarity with vector embeddings

#### Expected Outcome

By the end of the workshop, you will have:

* An agent capable of answering FAQs via vector search
* A memory system that remembers user preferences and context
* Tool orchestration allowing the agent to decide when to use tools
* Production-ready code with logging and error handling

---

**Get Started**: [5.1. Workshop Overview](5.1-workshop-overview/)

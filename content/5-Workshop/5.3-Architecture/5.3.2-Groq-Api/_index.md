---
title: "Calling Groq API"
weight: 3
chapter: false
pre: " <b> 5.3.2 </b> "
-----------------------

## Objective

Use the Groq library (`ChatGroq` / `init_chat_model` with `model_provider="groq"`) to call an OpenAI-compatible model hosted on Groq.

## Configuration in Code

In the demo code:

### Retrieve API Key from Environment

```python
GROQ_API_KEY = os.getenv("GROQ_API_KEY")
```

The variable `GROQ_API_KEY` loads the API key from the environment variable.

### Initialize the Model

```python
llm = init_chat_model(
    model="openai/gpt-oss-20b",
    model_provider="groq",
    api_key=GROQ_API_KEY
)
```

### Integrate with Agent

The Agent calls the LLM through `create_agent(...)` using the `model=llm` parameter:

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

## Processing Flow

```
Agent → Groq API → Model Inference → Response
```

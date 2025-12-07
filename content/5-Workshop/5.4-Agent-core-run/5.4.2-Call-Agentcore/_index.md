---

title : "Calling AgentCore"

weight : 1
chapter : false
pre : " <b> 5.4.2 </b> "
---
## Simple Demo with AgentCore

### 1. Send the first question

Use the command:

```bash
agentcore invoke "{'prompt': 'Tell me about roaming activations'}"
```

![test1](/aws/static/images/5-Workshop/5.4-S3-onprem/test1.png)

The Agent will respond based on the data you deployed (database + logic in your code).


### 2. Test memory between invocations (session)

After the first question, send another related one — for example:

```bash
agentcore invoke "{'prompt': 'Activate it for Vietnam'}"
```

![test2](/aws/static/images/5-Workshop/5.4-S3-onprem/test2.png)

Then ask:

```bash
agentcore invoke "{'prompt': 'which country was i referring to'}"
```

![test3](/aws/static/images/5-Workshop/5.4-S3-onprem/test3.png)

If the Agent responds correctly and remembers the previous information → this confirms the **Memory is working** and AgentCore is maintaining context across invocations.

AgentCore behaves as expected in this demo.


---

title: "Clean up"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
-----

#### Overview

In this section, we will remove everything that was created during this workshop.



### Deleting an Agent in AgentCore Runtime

1. Go to **Amazon Bedrock AgentCore** in the AWS Console.
2. In the left-hand menu, select **Build → Runtime**.
3. In the **Runtime resources** list, you will see the agents you have created, for example: `agent_demo_2` or `agent_with_memory`.
4. Select the agent you want to delete by ticking the radio button in the **Name** column.
5. Click the **Delete** button at the top right.
6. Confirm the deletion when AWS prompts you. The agent will be permanently removed from the Runtime resources list.


![Deleting an agent in Amazon Bedrock AgentCore](/images/5-Workshop/5.5-Clean/clean.png)



### Deleting a Memory in AgentCore

After deleting an agent in Runtime, you can manage or delete the **Memory** that the agent used:

1. In the left-hand menu, select **Build → Memory**.
2. In the Memory resources list, select the memory you want to delete.
3. Click the **Delete** button at the top right.
4. Confirm the deletion when AWS prompts you. The memory will be removed from the system.



![Deleting Memory in Amazon Bedrock AgentCore](/images/5-Workshop/5.5-Clean/clean_memory.png)

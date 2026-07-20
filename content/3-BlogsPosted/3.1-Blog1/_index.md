---

title: "Blog 1"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "

---
# BUILDING A SERVERLESS IMAGE EDITING AGENT WITH AMAZON BEDROCK AGENTCORE HARNESS

This post shares and walks through how to build a serverless image editing agent, where users upload a photo, describe the edit in plain English, and get the result back in seconds.

What makes this solution unique is the use of **Amazon Bedrock AgentCore Harness** to handle the entire AI agent orchestration process — including reasoning loop management, tool selection, conversation memory, and providing the execution environment.

AgentCore Harness runs the agent inside a stateful, isolated microVM, with memory, tool routing, and observability built in.

### How the application works

The AI agent in the application uses **Claude Sonnet 4.6** to break down an editing request into smaller steps. The agent then selects and invokes the tool that corresponds to the appropriate Stability AI model to perform the edit.

Once the image has been processed, a Python program runs directly inside the microVM to add a watermark. Since this step doesn't require AI reasoning, it consumes no tokens.

### Key capabilities

**1️⃣ Creating an AI agent through configuration**

The agent is defined entirely through API parameters — no need to write custom orchestration code, adopt a separate framework, or build a container.

**2️⃣ Switching models per request**

The frontend uses **Claude Haiku 4.5** for simple conversations and **Claude Sonnet 4.6** for complex image editing requests.

**3️⃣ Changing persona without redeploying**

Users can choose from domain-specific personas. Each persona provides a system prompt tailored to its domain, without needing to modify or redeploy the entire application.

**4️⃣ Storing history with AgentCore Memory**

AgentCore Memory retains conversation history for 30 days. This lets the agent reference previous edits without the frontend having to resend the entire conversation.

The session ID is stored in `localStorage`, so the conversation continues even after the user reloads the page. If browser data is cleared, the frontend creates a new session, but the previous history can still be retrieved from AgentCore through the **ListEvents API**.

**5️⃣ Connecting tools through MCP**

Three image editing tools are built with **AWS Lambda** and exposed to the agent through **AgentCore Gateway** and the **Model Context Protocol (MCP)**.

The agent decides which tool to use based on the content of the request.

**6️⃣ Handling non-AI tasks to save tokens**

After each edit, a Python program runs directly inside the microVM to add a watermark. Since this is a standard image-processing operation, it doesn't require a reasoning model and incurs no token cost.

### System architecture

* A **React** frontend hosted on **AWS Amplify**, where users upload images, draw a mask, and enter editing instructions.
* An **AWS Lambda** proxy server that acts as a security boundary between browser credentials and the system's API, while also controlling which system prompts are allowed.
* An **Amazon Bedrock AgentCore** agent with **AgentCore Memory** for storing the conversation.
* Three **Lambda** tool functions that call **Stability AI** foundation models through Amazon Bedrock to generate the images.

The diagram below illustrates the overall architecture of the solution, from the user interface to the AgentCore components and image-processing tools on AWS.

![Amazon Bedrock Architecture](/images/3-Blog/bedrock-architecture.png)

**Reference:**

https://aws.amazon.com/vi/blogs/machine-learning/build-a-serverless-image-editing-agent-with-amazon-bedrock-agentcore-harness/

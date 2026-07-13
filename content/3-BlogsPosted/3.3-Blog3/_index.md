---
title: "Blog 3"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
# WEB SEARCH IN AMAZON BEDROCK AGENTCORE

Large Language Models (LLMs) have become a core component of modern AI applications. However, one major limitation is that they can only answer questions based on the data available during training. When users ask about breaking news, current stock prices, newly released AI models, or recent AWS announcements, AI agents cannot provide accurate answers without access to external information.

To solve this challenge, AWS introduced **Web Search on Amazon Bedrock AgentCore**, a fully managed web search capability that enables AI agents to retrieve real-time information from the public web without integrating third-party search services.

## Key Features

* Enables AI agents to access real-time information from the public web.
* Eliminates the need for Google Search API, Bing Search API, or SerpAPI.
* AWS manages the entire search infrastructure and result processing.
* Supports **Model Context Protocol (MCP)** for seamless integration with AI frameworks.
* Works through **Amazon Bedrock AgentCore Gateway**.
* User queries remain entirely within AWS infrastructure, improving privacy and security.
* Uses **Semantic Snippet Extraction** to return only the most relevant information.
* Supports **Knowledge Graph** to improve factual accuracy for entity-based queries.

This feature is especially useful for AI chatbots, virtual assistants, research agents, financial assistants, news summarizers, and any AI application that requires continuously updated information.

---

## Architecture

The overall workflow consists of the following steps:

1. A user sends a request to an AI agent.
2. The agent determines that current information is required.
3. The request is forwarded to the Amazon Bedrock AgentCore Gateway.
4. The Gateway invokes the managed Web Search Tool.
5. AWS searches its Web Index and extracts relevant information.
6. Search results are returned to the AI agent.
7. The AI agent generates a grounded response with citations.

![Architecture](/images/3-Blog/image.png)

---

## Web Search Tool

Developers can enable Web Search simply by attaching a managed Web Search Tool to an existing AgentCore Gateway using the following connector:

```python
connectorId = "web-search"
```

Once configured, AI agents can automatically decide when to use Web Search to answer questions requiring fresh information.

---

## Amazon Web Index

Unlike many solutions that rely on third-party search providers, AWS maintains its own **Web Index**.

According to AWS, this index:

* Contains tens of billions of web documents.
* Is continuously updated within minutes.
* Provides broad coverage across the public Internet.
* Is optimized specifically for AI retrieval.

This enables AI agents to access more recent and reliable information.

---

## Semantic Snippet Extraction

Instead of returning raw HTML or an entire web page, Web Search extracts only the most relevant passages related to the user's query.

This approach provides several advantages:

* Reduces token consumption.
* Improves response quality.
* Increases retrieval accuracy.
* Removes unnecessary page content.

---

## Knowledge Graph

Web Search also leverages an integrated **Knowledge Graph** for entity-based questions.

For queries about:

* People
* Organizations
* Companies
* Locations

the Knowledge Graph provides structured factual information, helping reduce hallucinations and improve response reliability.

---

## Security

One of the major advantages of Amazon Bedrock AgentCore Web Search is its privacy-first architecture.

All search requests remain inside AWS infrastructure instead of being forwarded to external search engines.

Authentication is handled through IAM Roles attached to the AgentCore Gateway, simplifying credential management while improving enterprise security.

---

## Integration

Web Search follows the **Model Context Protocol (MCP)** standard.

It can be integrated with popular AI frameworks including:

* Strands
* LangChain
* LangGraph
* CrewAI

The AI agent automatically discovers available tools from the Gateway and invokes Web Search whenever real-time information is required.

---

## Pricing

According to AWS pricing:

* **USD $7 per 1,000 search requests**

The service follows a pay-as-you-go pricing model, allowing customers to pay only for actual usage.

---

## Conclusion

Amazon Bedrock AgentCore Web Search enables AI agents to overcome the limitations of static training data by providing secure access to real-time web information.

Instead of integrating multiple third-party search services, managing API keys, or building custom retrieval pipelines, developers only need to configure a managed Web Search Tool while AWS handles indexing, authentication, retrieval, and result optimization.

This feature is an excellent choice for intelligent chatbots, AI assistants, research platforms, financial applications, news summarization systems, and customer support solutions that require continuously updated information.

---

## References

**AWS Machine Learning Blog**

Introducing Web Search on Amazon Bedrock AgentCore

https://aws.amazon.com/blogs/machine-learning/introducing-web-search-on-amazon-bedrock-agentcore/
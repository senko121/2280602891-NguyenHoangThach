---
title: "Published Blog Posts"
date: 2026-07-05
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

During my internship, I proactively researched newly released AWS technologies and shared my findings through a series of technical blog posts on the **AWS Study Group** community. These blogs focus on newly introduced AWS features as well as practical solutions for building, operating, and optimizing modern AI applications using Amazon Bedrock and Amazon Bedrock AgentCore.

The published blog posts include:

### [Blog 1 - BUILDING A SERVERLESS IMAGE EDITING AGENT WITH AMAZON BEDROCK AGENTCORE HARNESS](3.1-Blog1/)

This blog walks through building a serverless image editing agent, where users upload a photo, describe the edit in plain English, and get the result back in seconds. It focuses on **Amazon Bedrock AgentCore Harness**, which handles the entire AI agent orchestration, conversation memory, tool routing through the Model Context Protocol (MCP), and an isolated microVM execution environment.

### [Blog 2 - OPTIMIZING NAT GATEWAY COSTS ON AWS](3.2-Blog2/)

This blog breaks down how AWS bills for NAT Gateway and why costs can spike quickly once a system spans multiple Availability Zones. It walks through identifying real traffic patterns using VPC Flow Logs, Amazon Athena, and CloudWatch, then applying an **S3 Gateway Endpoint** to remove most S3-bound traffic from the NAT Gateway path — cutting monthly operating costs significantly.

### [Blog 3 - WEB SEARCH IN AMAZON BEDROCK AGENTCORE](3.3-Blog3/)

This blog introduces the **Web Search** capability in Amazon Bedrock AgentCore, which enables AI Agents to retrieve real-time information from the public web without integrating third-party search services. It covers the overall architecture, Amazon Web Index, Semantic Snippet Extraction, Knowledge Graph, integration with popular AI frameworks, and the security and scalability benefits for modern AI applications.
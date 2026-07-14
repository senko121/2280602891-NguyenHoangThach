---
title: "Event 2"
date: 2026-05-12
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "FCAJ Community Day #2"

### Event Objectives

* Provide practical perspectives on the technology job market in the AI era.
* Share techniques for working effectively with LLMs: context engineering and understanding model limitations.
* Introduce modern AWS services: Amazon Quick Suite and Amazon CloudFront in system architecture.
* Inspire attendees through hackathon experiences and enterprise-grade multi-agent system design.

### Speakers

* **Nguyen Gia Hung** – Shared perspectives on the job market and AI.
* **Tinh Truong** (Platform Engineer) – "Context is Everything": context management when working with LLMs.
* **Nguyen Hai Anh** – "Friendly AI Assistant with Amazon Quick": introduction to Amazon Quick Suite.
* **Nguyen Tuan Thinh** – "CloudFront as Your Foundation": Amazon CloudFront as an architectural foundation.
* **UTM Morpho Team (Uyen Thao)** – Shared hackathon competition experiences.
* **Duc Dao** – "Non-determinism of 'Deterministic' LLM Settings": why LLMs are never fully deterministic.
* **Vy Lam** – "Enterprise-grade Multi-agent System": building production-ready multi-agent systems.

### Key Highlights

#### Perspectives on the job market and AI – Nguyen Gia Hung

The opening session provided an overview of the technology recruitment landscape as AI adoption accelerates.

* AI does not fully replace developers, but it is reshaping hiring standards: employers now expect candidates to use AI to increase productivity.
* Junior positions face stronger competition, making solid fundamentals and fast learning ability the most important advantages.
* Soft skills, problem-solving mindset and adaptability remain qualities that AI cannot replace.
* Advice for students: build a portfolio of real projects instead of only collecting certificates.

#### Context is Everything – Tinh Truong (Platform Engineer)

This session focused on Context Engineering — the decisive factor for output quality when working with LLMs.

* LLM response quality depends directly on the quality of the provided context, not only on prompt wording.
* The context window is a finite resource: information must be selected carefully instead of stuffing in everything.
* Context management techniques: conversation history summarization, retrieval of relevant documents (RAG), and structured input formatting.
* From a Platform Engineer's perspective, designing systems that feed the right context to AI matters more than fine-tuning individual prompts.

#### Friendly AI Assistant with Amazon Quick – Nguyen Hai Anh

This session introduced **Amazon Quick Suite** — AWS's new agentic AI application for the workplace.

* Amazon Quick Suite helps employees find insights, conduct deep research, visualize data and automate everyday tasks.
* Its ability to connect to multiple enterprise data sources lets the AI assistant answer from internal data instead of general knowledge only.
* A live demo showed how to build a friendly AI assistant serving lookup and reporting needs for non-technical teams.
* Key point: an effective AI assistant does not need to be complex — what matters is integrating it properly into existing workflows.

#### CloudFront as Your Foundation – Nguyen Tuan Thinh

This session analyzed the role of **Amazon CloudFront** as the foundational layer of modern web architecture.

* CloudFront is not just a CDN for content acceleration but also a secure entry point for the entire system.
* Combining CloudFront with S3 (via Origin Access Control) and ALB serves both static content and APIs through a single HTTPS domain.
* Foundational benefits: DDoS protection at the edge, WAF integration, caching to reduce origin load, and optimized data transfer costs.
* Architectural lesson: placing CloudFront as the foundation from day one makes the system more scalable and secure later — closely matching the architecture used in the PetShop workshop.

#### Hackathon experience sharing – UTM Morpho Team (Uyen Thao)

This session shared real experiences of a student team competing in hackathons.

* The 24–48 hour workflow: pick an idea quickly, divide work by strengths, and prioritize a demo-able product.
* Teamwork lessons under time pressure: concise communication, decisive choices and accepting feature scope trade-offs.
* Pitching to judges: focus on the problem being solved rather than listing the technologies used.
* Hackathons are the fastest way to learn: even losing brings valuable experience and connections.

#### Non-determinism of "Deterministic" LLM Settings – Duc Dao

This deep technical session explained why LLMs still produce different results even under "deterministic" configurations.

* Setting `temperature = 0` does not guarantee identical outputs across calls.
* Root causes come from the infrastructure layer: floating-point non-associativity, parallel computation ordering on GPUs, and batching mechanisms in model serving systems.
* Practical implication: systems should never rely on LLMs returning identical results; validation and divergence handling must exist at the application layer.
* Lesson: understanding a tool's limitations is just as important as knowing how to use it.

#### Enterprise-grade Multi-agent System – Vy Lam

The final session introduced how to build coordinated multi-agent AI systems that meet enterprise deployment standards.

* Multi-agent architecture: an orchestrator agent delegates tasks to specialized agents instead of one agent doing everything.
* Enterprise requirements: access control, full observability and logging, cost limits, and human-in-the-loop approval mechanisms.
* Practical challenges: managing shared context between agents, handling cascading failures, and automated output quality evaluation.
* Trend: multi-agent systems are the evolution from single chatbots to end-to-end automated business workflows.

### Key Takeaways

#### Career Orientation

* The job market changes fast, but solid fundamentals and adaptability remain the core values.
* Using AI effectively has become a default skill that employers expect.
* A portfolio of real projects is more convincing than a list of certificates.

#### AI Working Mindset

* Context Engineering determines output quality more than individual prompt wording.
* LLMs are non-deterministic even in "deterministic" settings — real systems must validate and handle output divergence.
* Multi-agent systems require enterprise constraints: security, observability, cost control and human approval.

#### AWS System Architecture

* CloudFront should be treated as the foundation of web architecture: acceleration, security and a unified entry point at once.
* Amazon Quick Suite opens a path to building internal AI assistants connected directly to enterprise data.

#### Soft Skills

* Teamwork under time pressure requires concise communication and fast decisions.
* Pitching should focus on the value of the problem solved rather than technical details.

### Applying to Work

* Apply **Context Engineering** thinking when working with AI: prepare documents and context before asking, instead of only tweaking prompts.
* Reinforce the **CloudFront as single entry point** architecture in the PetShop project and future projects.
* Add **LLM output validation** steps in AI-integrated applications; never assume consistent results.
* Explore and experiment with **Amazon Quick Suite** for automated lookup and reporting needs.
* Join hackathons to practice teamwork, time management and product pitching skills.

### Event Experience

Attending **"FCAJ Community Day #2"** exposed me to a diverse range of topics — from career orientation and deep LLM internals to AWS system architecture.

#### Learning from experienced speakers

Seven presentations from speakers in different roles — Platform Engineer, AI engineers, solution architects and a student hackathon team — offered a multi-dimensional view of the technology industry.

The combination of orientation topics and technical topics helped me both broaden my perspective and learn immediately applicable knowledge.

#### Deeper understanding of LLM fundamentals

The two sessions on Context Engineering and LLM non-determinism changed how I view AI: instead of treating AI as an always-correct black box, I learned to understand model limitations and design workflows accordingly.

#### Connecting knowledge with practice

The CloudFront session was especially valuable because it matched the architecture I deployed in the PetShop workshop — hearing an expert's analysis deepened my understanding of the architectural decisions I had already applied.

#### Networking and discussions

The event provided opportunities to talk directly with speakers and fellow attendees; the hackathon sharing in particular inspired me to confidently join upcoming technology competitions.

#### Lessons learned

* Context is the most important factor when working with LLMs — "Context is Everything".
* Understanding technology limitations is just as important as knowing how to use it.
* Good architecture starts from the right foundation — CloudFront is a prime example of this mindset.
* The AI-era job market favors people with strong fundamentals and fast learning ability.
* Hands-on experience (hackathons, real projects) is the most effective and convincing way to learn.

#### Some event photos

*Add your event photos here*

> Overall, the event provided both deep technical knowledge and a clearer career direction in the AI era. The sessions on Context Engineering, LLM limitations and AWS architecture are directly applicable to my studies and real projects, while the job market and hackathon sharing gave me more confidence on my career path in information technology.

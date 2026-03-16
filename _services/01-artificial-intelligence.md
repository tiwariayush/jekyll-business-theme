---
title: "AI & LLM Solutions"
date: 2018-11-18T12:33:46+10:00
weight: 1
description: "LLM integration, agentic workflows, RAG pipelines, and Bayesian inference in PostgreSQL. AI systems engineered for production, not just demos."
---

## The hard part isn't the model

Everyone has access to GPT-4, Claude, Mistral. The API call is the easy part. The hard part is everything around it: getting the model to stop hallucinating on your specific domain, keeping costs from spiraling when you move from 10 test queries to 10,000 real users, building fallbacks for when the API is slow or down, and figuring out how to measure whether the thing is actually working well enough to trust.

That's the work we do. Not wrapping an API in a UI and calling it AI, but the real engineering of making language models, agents, and probabilistic systems behave reliably in production environments.

## What this looks like in practice

**When a company comes to us wanting to add AI to their product**, the first conversation is never about which model to use. It's about what decision they're trying to make better, or what workflow they're trying to automate, and whether AI is genuinely the right tool for it. Sometimes it's not, and we'll tell you that.

When it is the right tool, we typically work through a few layers:

**Understanding your data first.** Most AI projects fail not because the model is wrong but because the data going into it is messy, incomplete, or structured in a way the model can't use. We spend real time here, understanding your data model, what's actually in your database vs. what you think is in it, and what shape it needs to be in.

**Building a working prototype quickly.** Not a slide deck, not a Figma mockup, but a prototype that runs against your actual data and produces real outputs you can evaluate. If we're building a RAG system over your internal documentation, you'll be asking it questions within the first two weeks and seeing where it nails the answer and where it falls apart. That's when the interesting engineering starts.

**The grind of making it production-ready.** This is where we spend most of our time and where most projects stall without experienced help. It means building proper evaluation pipelines so you can measure quality across hundreds of test cases, not just the five you tried by hand. It means handling the edge cases: what happens when the user asks something completely off-topic, or when the context window fills up, or when the embedding search returns irrelevant results. It means setting up monitoring so you know when quality degrades before your users do.

## Solutions we deliver

These are the concrete things clients walk away with. Not strategy decks or "AI readiness assessments," but working systems.

### Knowledge base and document Q&A

The most common project we get asked about. You have internal documentation, policies, product manuals, support tickets, contracts, whatever, and you want people to be able to ask questions and get accurate answers grounded in that content.

What this actually involves: ingesting your documents, figuring out the right way to split and index them (this varies a lot depending on whether you're dealing with short support articles or 200-page technical manuals), choosing an embedding model that works for your domain, wiring up retrieval and generation, and then the critical part that most teams skip: building evaluation so you can prove the system gives correct answers and catch the cases where it doesn't. You end up with a Q&A system you can embed in your product, your internal tools, or a standalone interface, with a dashboard showing answer quality over time.

### AI features inside your existing product

You already have a product and you want to add intelligence to it. Maybe it's a writing assistant, a smart search that understands intent instead of just keywords, an automatic categorization system, or a feature that summarizes long content for your users.

The tricky part here isn't the AI itself, it's integrating it cleanly into your existing codebase and UX without creating a slow, expensive, or unreliable experience. We handle the backend integration (API design, caching, fallbacks), the prompt engineering, and the frontend work to make it feel like a natural part of your product. We pay close attention to latency and cost, because an AI feature that takes 8 seconds to respond or costs you €0.10 per request isn't going to survive contact with real users.

### Workflow automation with agents

You have a process that involves a person collecting information from one system, making a decision, updating another system, and maybe sending a notification. It happens dozens or hundreds of times a day, it follows a roughly predictable pattern, but it has enough edge cases that a simple script won't cut it.

This is where agentic systems earn their keep. We build agents that can pull data from your CRM, database, or APIs, reason through the logic, take actions, and escalate to a human when they're uncertain. The key design choice is always: where does the agent act autonomously, and where does it stop and ask? We set those boundaries carefully, build logging so you can audit every decision the agent made, and include a human override for the cases where the agent gets it wrong. Because it will get things wrong sometimes, and the system needs to handle that gracefully.

### Document processing and data extraction

Invoices, contracts, forms, emails, PDFs, whatever. You receive unstructured documents and need structured data out of them. The old approach was rule-based parsing, which breaks every time the format changes slightly.

We combine traditional parsing (for the predictable parts) with LLM-based extraction (for the parts that vary). The system reads a document, pulls out the fields you care about, validates them against your expected schema, flags anything it's unsure about for human review, and writes the structured result to your database. For high-volume use cases (thousands of invoices per month, for example), we optimize for cost by using smaller models where possible and only escalating to more capable models when needed.

### Decision systems under uncertainty

This is where our Bayesian research background comes in. Some problems aren't about generating text or automating workflows. They're about making better decisions when you don't have complete information.

How confident should you be in a product recommendation? Is this transaction likely fraudulent, and how sure are you? What's the probability that demand will spike next quarter, and what's the range? Standard ML models give you a single number. Bayesian approaches give you a distribution, which means you know not just the answer but how much to trust it. We've built [these algorithms to run directly inside PostgreSQL](https://github.com/cartesiantrees/postgres-bayes), so the reasoning happens where your data already lives. This matters for recommendation engines, risk scoring, anomaly detection, and any scenario where acting on a wrong prediction has real consequences.

### AI monitoring and evaluation

Maybe you already shipped an AI feature. It worked well initially, but now you're not sure. Users are complaining about bad answers, costs have crept up, and you have no way to tell whether the system is getting better or worse over time.

We build the observability layer: automated evaluation suites that test your system against real examples on a schedule, dashboards that track accuracy, relevance, latency, and cost trends, alerts that fire when quality dips below your threshold, and tooling that makes it easy to investigate specific failures. Think of it as the monitoring and alerting you'd build for any production service, but adapted for the specific failure modes of AI systems (hallucination, drift, context window overflow, embedding quality degradation).

## The part nobody wants to talk about: cost and evaluation

Two things kill AI projects after launch: uncontrolled costs and no way to measure quality.

On **cost**: a naive integration with GPT-4 can easily run up thousands of euros per month once you have real traffic. We architect systems with cost in mind from the start. Caching, model routing (use a cheaper model for easy queries, a powerful one for hard ones), prompt optimization to reduce token usage, and monitoring that alerts you before the bill surprises you.

On **evaluation**: if you can't measure it, you can't improve it. We build evaluation frameworks specific to your use case. Automated test suites that run against real examples, metrics that track accuracy, relevance, and groundedness over time, and dashboards that show you trends. When quality drops (and it will, as your data changes and models update), you'll know immediately.

If you're trying to figure out how AI fits into your product, or you've started and hit a wall, [let's talk](/contact). We're particularly useful when you've moved past the "let's experiment" phase and need someone who can actually ship it.

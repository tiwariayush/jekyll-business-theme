---
title: "AI & LLM Solutions"
date: 2018-11-18T12:33:46+10:00
weight: 1
description: "We build AI systems that work in production — LLM-powered apps, agentic workflows, RAG over proprietary data, and Bayesian inference in PostgreSQL. Paris-based, JEI-certified."
---

## The hard part isn't the model

Everyone has access to GPT-4, Claude, Mistral. The API call is the easy part. The hard part is everything around it: getting the model to stop hallucinating on your specific domain, keeping costs from spiraling when you move from 10 test queries to 10,000 real users, building fallbacks for when the API is slow or down, and figuring out how to measure whether the thing is actually working well enough to trust.

That's the work we do. Not wrapping an API in a UI and calling it AI — but the real engineering of making language models, agents, and probabilistic systems behave reliably in production environments.

---

## What this looks like in practice

**When a company comes to us wanting to add AI to their product**, the first conversation is never about which model to use. It's about what decision they're trying to make better, or what workflow they're trying to automate, and whether AI is genuinely the right tool for it. Sometimes it's not, and we'll tell you that.

When it is the right tool, we typically work through a few layers:

**Understanding your data first.** Most AI projects fail not because the model is wrong but because the data going into it is messy, incomplete, or structured in a way the model can't use. We spend real time here — understanding your data model, what's actually in your database vs. what you think is in it, and what shape it needs to be in.

**Building a working prototype quickly.** Not a slide deck, not a Figma mockup — a prototype that runs against your actual data and produces real outputs you can evaluate. If we're building a RAG system over your internal documentation, you'll be asking it questions within the first two weeks and seeing where it nails the answer and where it falls apart. That's when the interesting engineering starts.

**The grind of making it production-ready.** This is where we spend most of our time and where most projects stall without experienced help. It means building proper evaluation pipelines so you can measure quality across hundreds of test cases, not just the five you tried by hand. It means handling the edge cases — what happens when the user asks something completely off-topic, or when the context window fills up, or when the embedding search returns irrelevant results. It means setting up monitoring so you know when quality degrades before your users do.

---

## Areas where we go deep

**LLM-powered applications** — We've integrated OpenAI, Anthropic, and Mistral models into production systems. The real skill isn't the API call; it's the prompt architecture, the output validation, the retry logic, the cost monitoring, and the evaluation framework you build around it. We know the failure modes because we've hit them.

**Agentic workflows** — AI agents that can take multi-step actions, use tools, and make decisions. The challenge is reliability: agents fail in creative ways, and you need proper error recovery, logging, and human-in-the-loop checkpoints. We build these with observability from day one so you can see exactly what the agent did, what it decided, and why.

**RAG systems** — Most RAG implementations we've seen in the wild perform poorly because the chunking is wrong, the embedding model is generic, or the retrieval step returns quantity over relevance. We tune the full pipeline: how documents are split, which embedding model fits your domain, how retrieval is scored, and how the final response is generated and grounded.

**Bayesian inference and probabilistic reasoning** — This is our research background. We've built [Bayesian inference algorithms that run directly inside PostgreSQL](https://github.com/cartesiantrees/postgres-bayes) — MCMC, Gibbs sampling, variational inference — because we believe probabilistic reasoning should live where the data lives, not in a separate Python notebook. When you need a system that quantifies uncertainty (how confident are we in this recommendation? how likely is this transaction to be fraud?), this is the approach that gives you honest answers instead of just a number.

---

## The part nobody wants to talk about: cost and evaluation

Two things kill AI projects after launch: uncontrolled costs and no way to measure quality.

On **cost**: a naive integration with GPT-4 can easily run up thousands of euros per month once you have real traffic. We architect systems with cost in mind from the start — caching, model routing (use a cheaper model for easy queries, a powerful one for hard ones), prompt optimization to reduce token usage, and monitoring that alerts you before the bill surprises you.

On **evaluation**: if you can't measure it, you can't improve it. We build evaluation frameworks specific to your use case — automated test suites that run against real examples, metrics that track accuracy, relevance, and groundedness over time, and dashboards that show you trends. When quality drops (and it will, as your data changes and models update), you'll know immediately.

---

If you're trying to figure out how AI fits into your product — or you've started and hit a wall — [let's talk](/contact). We're particularly useful when you've moved past the "let's experiment" phase and need someone who can actually ship it.

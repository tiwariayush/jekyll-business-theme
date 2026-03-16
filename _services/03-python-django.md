---
title: "Python & Web APIs"
date: 2018-11-18T12:33:46+10:00
weight: 3
description: "Python backends and APIs with Django and FastAPI. The infrastructure behind web apps, AI systems, payment flows, and data pipelines."
tags: ["Python", "Django", "FastAPI", "PostgreSQL", "Stripe", "REST"]
tagline: "Backends and APIs that power AI systems and payments."
---

![Python and Django](/images/pixeltrue-web-development-1.svg)

## The backend is where the real work happens

A well-designed API is invisible. Users don't see it, designers don't think about it, and business stakeholders rarely ask about it. But it's the thing that determines whether your application is fast or slow, whether it can handle growth, whether it's secure, and whether your team can build on top of it without wanting to rewrite everything in six months.

We build backends in Python, primarily with Django and FastAPI, and we've been doing it long enough to have strong opinions about how to do it well.

## Django or FastAPI? Depends on the problem.

We don't have a religious preference. Django is the right choice when you need a full-featured application with authentication, admin interfaces, ORM, and a mature ecosystem of packages, which describes most business applications. FastAPI is the right choice when you need raw speed, async capabilities, and automatic OpenAPI documentation, which describes most AI serving endpoints and high-throughput APIs.

Sometimes we use both in the same project. Django for the main application, FastAPI for the performance-critical inference endpoints. The choice should follow from the problem, not the other way around.

## What we've actually built

We're not going to list "Authentication" and "Rate Limiting" as services. Those are just things that competent backends have. Instead, here's the kind of work we do:

**APIs that serve AI models.** When you've got a trained model that needs to serve predictions to real users, someone has to build the API around it. That means input validation, async processing for slow inference, proper error handling when the model fails, response caching, cost-aware routing between different models, and monitoring that tells you latency percentiles, not just averages. We've done this with LLM-powered systems, Bayesian inference engines, and traditional ML models.

**Payment and billing infrastructure.** We've contributed to the [Stripe API ecosystem](https://github.com/stripe) and built payment integrations from scratch: subscription management, metered billing, webhook handling, idempotent charge creation. Payment code has zero tolerance for bugs, and we write it accordingly. Extensive test coverage, careful error handling, and audit logging for every transaction.

**Data ingestion pipelines.** APIs that receive data from external sources (webhooks from third-party services, file uploads, streaming data), validate it, transform it, and store it correctly. The hard part is usually the edge cases: what happens when the webhook comes twice, when the file is malformed, when the external service changes their payload format without telling you.

**Internal tools and admin systems.** Django's admin is remarkably powerful if you know how to extend it. We've built custom admin interfaces that let non-technical team members manage complex business logic (content moderation queues, order management systems, user account tooling) without developers needing to be involved in day-to-day operations.

## How we think about API design

Good API design is about making the next developer's life easier, whether that's your frontend team, a third-party integration partner, or your future self. A few things we care about:

**Consistency over cleverness.** Every endpoint should behave the same way: same error format, same pagination style, same authentication pattern. We follow REST conventions where they make sense and deviate only when there's a good reason.

**Fail loudly with useful errors.** A 500 error with no context is hostile to whoever is trying to integrate with your API. We design error responses that tell you what went wrong, where, and ideally how to fix it.

**Tests that catch real bugs.** Not tests that cover lines of code for the sake of metrics, but tests that exercise the actual workflows your API supports, including the unhappy paths. We write integration tests that hit the database, test the authentication flow end-to-end, and verify that your payment webhook handler does the right thing when Stripe sends a duplicate event.

If you need a backend built, an API redesigned, or an existing system that's becoming hard to maintain, [reach out](/contact). We're particularly good at projects where reliability matters: payment systems, AI infrastructure, and data pipelines where silent failures are not an option.

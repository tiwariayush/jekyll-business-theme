---
title: "Personal Assistant"
weight: 2
description: "An AI-powered life management system for busy professionals. Voice capture, multi-agent task management, smart scheduling, and proactive assistance, built with React Native and FastAPI."
repo: "https://github.com/tiwariayush/pa"
status: "In development"
stack: "React Native, FastAPI, PostgreSQL, Redis, GPT-4/Claude"
license: "Open Source"
---

## Intelligent task management that actually thinks

Personal Assistant is a system we're building to solve a problem we kept running into ourselves: managing the constant stream of tasks, ideas, and commitments across work and personal life. The standard to-do app doesn't cut it when you need context-aware prioritization, smart scheduling around your existing calendar, and proactive help with research and planning.

## How it works

The system is built around a multi-agent AI architecture where different specialized agents handle different concerns:

**Voice capture with intelligent parsing.** You talk, the system listens, and it doesn't just transcribe. It extracts the task, figures out the priority, guesses the time estimate, assigns it to the right domain (work vs. personal vs. family), and suggests when to do it based on your schedule and energy patterns.

**Context-aware prioritization.** Not just "high/medium/low" priority. The system considers what time it is, where you are, how much energy you likely have, what's on your calendar next, and what deadlines are approaching. "What should I do right now?" gets an answer that's actually useful.

**Smart scheduling.** Connects to Google Calendar and Microsoft Calendar, finds open blocks, and creates time blocks for tasks that need focused work. Respects your patterns (don't schedule deep work right after lunch, keep mornings free for creative tasks) and adjusts when things shift.

**Proactive assistance.** The AI agents don't just wait for commands. If you have a meeting tomorrow, the research agent can prepare a briefing. If you've been putting off an email, the email agent can draft it. If a project has been stalled, it surfaces that.

## Architecture

```
Mobile App (React Native)
    ↕
Backend API (FastAPI)
├── User/Auth Service
├── Task & Project Service
├── Calendar Integration Service
├── AI Orchestrator Service
└── Notification Service
    ↕
External Services
├── STT/TTS Provider
├── LLM Provider (GPT-4/Claude)
├── Google/Microsoft Calendar APIs
└── Search & Research APIs
```

## The mobile app

Built in React Native for iOS and Android. Five main screens:

- **Today**: "What should I do now?" with smart recommendations based on your context
- **Inbox**: Voice captures with AI-parsed structure and one-tap confirmation
- **Tasks**: Organized by domain (work, personal, family) with priority scoring
- **Calendar**: Integrated view with task blocks and drag-and-drop scheduling
- **Assistant**: Chat interface for planning, research, and "help me think through this" conversations

## Current status

In active development. Phase 1 (core capture and task management) is functional. We're working through Phase 2 (calendar integration and smart scheduling) now, with email integration and proactive learning planned for Phase 3 and 4.

[View on GitHub](https://github.com/tiwariayush/pa)

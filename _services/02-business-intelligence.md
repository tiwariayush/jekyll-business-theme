---
title: "Business Intelligence"
date: 2018-11-18T12:33:46+10:00
weight: 6
description: "Metabase setup and BI infrastructure your whole team can actually use, not just the engineers. Dashboards, data modeling, and source integration done right."
tags: ["Metabase", "PostgreSQL", "Dashboards", "SQL", "Data modeling"]
tagline: "Metabase setup your whole team can actually use."
---

## Your data is probably more useful than you think

Most companies sit on data they never look at. Not because they don't care, but because the tooling is wrong. Either the BI system requires SQL knowledge that only two people on the team have, or the dashboards were set up once by a consultant and nobody knows how to change them, or the data sources aren't connected properly so the numbers don't match what's in the database.

We fix that. We work with Metabase, and we don't just implement it. We're [open-source contributors to the project](https://github.com/metabase/metabase). When we set up your BI infrastructure, we understand the tool at the code level, not just the configuration level.

## Why Metabase

We've worked with various BI tools and keep coming back to Metabase for most clients. It's open source, which means no per-seat licensing costs that scale with your team. It's self-hosted, so your data stays on your infrastructure. And most importantly, it's genuinely usable by non-technical people. Your marketing team, your ops team, your CEO can ask questions of the data without writing SQL.

That said, Metabase only works well if it's set up well. A bad Metabase installation is just a slow website with confusing charts. A good one becomes the place your team goes every morning to understand what's happening in the business.

## What we actually do

**Connect your data sources properly.** This sounds simple but it's where most BI projects go wrong. You've got a PostgreSQL database, maybe a data warehouse, some data in third-party APIs, maybe spreadsheets that someone insists on keeping. We connect everything into a unified view, handle the data modeling so that joins are correct and performant, and set up sync schedules so the data is fresh without overloading your database.

**Build dashboards that people use.** The difference between a dashboard that gets used daily and one that gets forgotten is design. Not visual design, but information design. Which metrics actually matter for which team? What's the right granularity? What filters do people need? We build dashboards iteratively with the people who'll use them, not in isolation.

**Set up the data model.** Metabase works best when the underlying data model is clean: proper table relationships, sensible naming, pre-computed metrics where needed. We do this work in the database layer and in Metabase's model configuration, so that when someone asks "what was our revenue last quarter by region," the answer comes back fast and correct.

**Train your team to be self-sufficient.** We're not interested in being your permanent BI department. We set things up, show your team how to create their own questions and dashboards, document the data model so new hires can understand it, and then step back. You should need us for the hard stuff (new data source integrations, performance problems, major schema changes), not for adding a filter to a dashboard.

If you've got data in a database and a team that's making decisions based on gut feeling or messy spreadsheets, [let's talk](/contact). The setup is usually faster than people expect.

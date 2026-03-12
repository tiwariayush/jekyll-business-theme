---
title: "Tracking & Measurement"
date: 2018-11-18T12:33:46+10:00
weight: 5
description: "Measurement infrastructure that survives ad blockers, privacy regulations, and browser changes. GTM, server-side tracking, and attribution you can trust."
---

## Measurement is an engineering problem now

Tracking used to be simple: drop a pixel on your page, watch the conversions roll in. That world is gone. Between iOS ATT, GDPR consent requirements, browser ITP, ad blockers, and the slow death of third-party cookies, the marketing team's carefully constructed attribution model is probably missing a significant chunk of reality.

The fix isn't more pixels — it's better architecture. Server-side tracking, proper consent management, first-party data strategies, and measurement setups that degrade gracefully when the browser won't cooperate. This is engineering work, not marketing work, and that's why we're the ones doing it.

---

## What's probably wrong with your current setup

If your tracking was set up more than two years ago and hasn't been seriously revisited, here's what's likely happening:

**Your numbers don't add up.** Google Analytics says one thing, Facebook Ads Manager says another, and your actual sales database says something different from both. The discrepancies aren't small — they can be 30-50%. This happens because each platform counts differently, consent blocks fire inconsistently, and events get lost or double-counted along the way.

**Your GTM container is a mess.** Someone added tags over the years, nobody removed the old ones, there are triggers firing that nobody understands, and the whole thing is fragile enough that a website redesign breaks half your tracking. We've audited GTM containers with 80+ tags where fewer than half were actually needed.

**You're not consent-compliant.** GDPR and ePrivacy require actual consent before firing tracking pixels in the EU. If your consent banner is cosmetic — loads the trackers regardless of what the user clicks — you're exposed to fines and your data is legally questionable anyway.

---

## How we approach it

**Audit first.** Before we change anything, we map what's currently firing on your site. Every tag, every pixel, every script. We compare what's supposed to be tracked against what's actually being tracked, identify gaps and redundancies, and document everything. This usually takes a few days and the results are consistently surprising.

**Clean GTM architecture.** We rebuild or restructure your Google Tag Manager setup with clear naming conventions, organized folders, proper trigger logic, and a data layer that your developers can feed from the application code. A good GTM setup should be maintainable by someone who didn't build it — that's the bar we aim for.

**Server-side where it matters.** For critical conversions — purchases, signups, high-value actions — we implement server-side tracking alongside client-side. This means the event fires from your server regardless of what's happening in the browser. We tie this into [Facebook's Conversion API](/services/04-facebook-capi/), Google Ads' enhanced conversions, and any other platform you're running ads on.

**Consent that actually works.** We implement consent management that respects user choices and actually blocks tracking when it should. This means integrating your consent platform (Cookiebot, OneTrust, or similar) with GTM so that tags only fire after consent is given for the correct category. Not just legally compliant — technically correct.

**Measurement you can trust.** Once the infrastructure is solid, we set up monitoring: automated checks that verify your key conversion events are still firing, alerting when event volume drops unexpectedly (which usually means something broke), and regular reconciliation between your tracking data and your actual database.

---

Tracking infrastructure is one of those things that quietly degrades until someone notices the numbers look wrong. If you suspect that's happening — or if you're about to launch campaigns and need to trust your data — [let's set it up properly](/contact).
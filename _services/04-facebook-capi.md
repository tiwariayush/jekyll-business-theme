---
title: "Facebook CAPI"
date: 2018-11-18T12:33:46+10:00
weight: 4
description: "iOS 14 and ad blockers broke your Facebook tracking. The Conversion API fixes it: server-side events, pixel migration, deduplication, and attribution that works."
---

## Your Facebook tracking is probably broken

If you're still relying primarily on the Facebook pixel for conversion tracking, you're likely missing 20-40% of your actual conversions. iOS 14's App Tracking Transparency, browser ad blockers, and ITP restrictions have systematically degraded browser-based tracking since 2021. The numbers in your Ads Manager don't match reality, and your ad optimization is suffering because of it.

Facebook's Conversion API (CAPI) is the fix. Instead of relying on a JavaScript pixel in the user's browser, CAPI sends conversion events directly from your server to Facebook's servers. No browser in the middle means no ad blockers, no iOS restrictions, no cookie limitations.

We've been implementing CAPI since it launched. We're contributors to the [Facebook Marketing API ecosystem](https://github.com/tiwariayush), and we've migrated tracking infrastructure for clients who were hemorrhaging attribution data.

## What a proper CAPI implementation looks like

A lot of agencies treat CAPI as "just add another tag." It's not. A proper implementation requires backend engineering, and getting it wrong can be worse than not having it at all. Duplicate events inflate your numbers, missing parameters degrade match quality, and incorrect event timing throws off attribution.

Here's what we actually work through:

**Server-side event architecture.** CAPI events need to be sent from your server, which means your backend needs to know when a user completes a purchase, signs up, adds to cart, or takes any other action you care about. We integrate this into your existing backend (whether it's Django, FastAPI, Node, or whatever you're running) so events fire reliably as part of your normal request flow, not as an afterthought.

**Event deduplication.** If you're running both the pixel and CAPI (which Facebook recommends), you need proper deduplication so the same conversion isn't counted twice. This means generating consistent event IDs on both client and server side. We've seen campaigns where cost-per-acquisition appeared to halve overnight after CAPI was added, not because performance improved, but because every conversion was being double-counted. Getting deduplication right is non-negotiable.

**Parameter quality and match rates.** CAPI's effectiveness depends on how well Facebook can match your server events to user profiles. That means sending hashed emails, phone numbers, IP addresses, user agents, and click IDs with proper formatting. The difference between a 30% match rate and an 80% match rate is the difference between CAPI being useful and being a waste of engineering time. We optimize every parameter.

**Pixel-to-CAPI migration.** For clients still running legacy pixel-only setups, we handle the full migration: audit existing pixel events, map them to CAPI equivalents, implement server-side sending, set up deduplication, and validate everything in Facebook's Event Manager before cutting over. We do this incrementally, not as a "big bang" migration that breaks your tracking for a week.

**Testing and validation.** Facebook's Event Manager and Test Events tool are useful but limited. We set up proper end-to-end testing: triggering real events through your site, verifying they arrive in Facebook's systems with correct parameters, checking match quality scores, and comparing server-side data with client-side data to ensure deduplication is working.

## Why this matters for your ad spend

Bad tracking doesn't just mean bad reports. It means bad optimization. Facebook's ad delivery algorithm optimizes towards conversions. If it can only see 60% of your actual conversions, it's making delivery decisions with incomplete data. It might be showing your ads to the wrong audiences, or it might be under-bidding because it thinks your conversion rate is lower than it actually is.

Clients who move to a properly implemented CAPI setup typically see improved match rates, better attribution accuracy, and (over a few weeks as Facebook's algorithm recalibrates) lower cost per acquisition. The improvement varies, but we've seen 15-30% reductions in CPA after proper implementation.

If your Facebook ad performance has degraded since iOS 14 and you suspect tracking is the issue, [let's talk](/contact). This is a well-scoped problem with a clear fix. Most implementations take 2-4 weeks depending on the complexity of your backend.

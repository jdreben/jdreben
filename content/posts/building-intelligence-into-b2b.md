---
title: "Selling to Machines: Engineering Intelligence Tools for B2B"
date: 2025-04-03
description: "On building software that makes sales and growth teams smarter — what I learned shipping revenue intelligence and product analytics tools for B2B companies."
tags: ["full-stack", "B2B", "product engineering", "data"]
---

For a stretch of my career I was building software to help sales teams understand what their users were actually doing in the product. Not what they said in calls. Not what the CRM recorded after the fact. What they were doing, in real time, before anyone had a conversation with them.

The category goes by a few names: revenue intelligence, product-led growth tooling, product analytics with a sales lens. The label doesn't really matter. The idea is that behavioral data from your product is a signal, and most B2B companies are not reading it well.

I was on a team trying to fix that.

## What the Problem Actually Is

In a product-led growth motion, users find and adopt your product before they ever talk to a salesperson. They try the free tier, explore features, hit limits, loop in teammates. All of that generates a behavioral trail. If you can read it correctly you can tell which accounts are expanding organically, which ones are stuck and likely to churn, which free users look like they're heading toward a real conversation.

That's enormously useful for a sales team. The hard part is surfacing it in a form that's actually actionable and not just a data dump.

## The Engineering Problem Is Layered

What looks like a product analytics problem is really a bunch of stacked engineering challenges.

**Event collection at scale.** You're instrumenting client applications to emit behavioral events: clicks, feature activations, API calls. At any real company that's millions of events a day. The pipeline that collects, validates, deduplicates and stores them has to be reliable, because data quality problems downstream are expensive and hard to diagnose retroactively. I spent more time on event pipelines than I expected. Worth it every time.

**Feature engineering that mirrors business concepts.** Raw events don't mean anything to a salesperson. "User fired `feature_x_activated` 14 times in the last 30 days" is noise. "User is a power user of the collaboration features" is signal. Getting from one to the other requires understanding the product well, and encoding that understanding into a pipeline that runs continuously on fresh data.

**Presentation that actually changes behavior.** A dashboard that requires sales reps to learn a new mental model won't get used. I've seen technically sophisticated products fail because the interface asked too much, and simple products succeed because the right thing was obvious. The frontend work on these products isn't glamorous but it's probably the most consequential part of the whole thing.

**Freshness.** A signal that tells you an account is heating up only matters if you have it in time to act. That pushes toward lower-latency pipelines, which push toward harder tradeoffs. Batch processing is simpler and cheaper. Streaming is faster and more complex. The right answer depends on the sales motion, which you can only really learn by watching salespeople actually use the thing you built.

## B2B Is Its Own Discipline

B2B has a specific dynamic: the people evaluating whether to buy your product are usually not the people who use it day to day. You have to make it good enough that end users adopt it organically, while making the outcomes visible enough that buyers can justify the purchase.

That tension shows up in engineering decisions. Audit logs, usage reports, admin dashboards, SSO and SCIM support: these features are invisible to users and matter enormously to buyers. They're not fun to build. They close enterprise deals. Learning to prioritize them is part of becoming a complete B2B engineer.

I also learned that schema design is basically destiny in B2B SaaS. The data model you commit to in the first six months shapes what you can actually build for the next few years. Getting it wrong is expensive, not immediately but gradually, as every new feature has to fight against a model that wasn't designed for it. The temptation is always to optimize for whats urgent right now. The discipline is holding enough of the long-term in your head to not close off doors you'll want later.

## Working With Sales Teams

Engineers and salespeople think really differently. Sales runs on intuition, relationship, and urgency. Engineering runs on evidence, abstraction, and completeness. Both are right. Both are limiting taken too far.

The most useful thing I picked up was getting comfortable with ambiguity around whether something worked. A feature that improves conversation quality doesn't show up clean in an A/B test. A workflow that saves thirty minutes per rep per week doesn't announce itself. Learning to find leading indicators that pointed toward value before the lagging ones showed up made me a better product engineer, not just for B2B.

---
title: "Selling to Machines: Engineering Intelligence Tools for B2B"
date: 2025-04-03
description: "On building software that makes sales and growth teams smarter — what I learned shipping revenue intelligence and product analytics tools for B2B companies."
tags: ["full-stack", "B2B", "product engineering", "data"]
---

At some point I spent a significant stretch of my career building software designed to help sales teams understand what their product's users were actually doing. Not what they said they were doing in calls, not what the CRM recorded after the fact — what they were *actually* doing, in the product, before anyone had a conversation with them.

The category is sometimes called revenue intelligence, sometimes product-led growth (PLG) tooling, sometimes just "product analytics with a sales lens." The label matters less than the underlying idea: behavioral data from your product is a signal, and most B2B companies are not listening to it well.

I was part of a team trying to fix that.

## The Problem Worth Solving

In a product-led growth motion, users discover and adopt a product before they ever speak to a salesperson. They try the free tier, explore features, hit limits, share access with colleagues. All of this generates a behavioral trail. If you can read that trail correctly, you can tell:

- Which accounts are expanding organically (and might convert to paid without a touch)
- Which accounts are getting stuck (and might churn if nobody intervenes)
- Which accounts have a power user who could be coached into a champion
- Which free users look like they're building toward a conversation

This is enormously useful information for a sales team. The challenge is surfacing it in a form that's actionable — not a data dump, but a signal.

## The Engineering Problem

What sounds like a simple product analytics problem turns out to be a layered engineering challenge.

**Event collection at scale.** You're instrumenting client applications to emit behavioral events — clicks, feature activations, time spent, API calls. At any meaningful company, that's millions of events per day. The pipeline that collects, validates, deduplicates, and stores them has to be reliable, because data quality problems downstream are hard to diagnose and expensive to fix retroactively. I spent more time on event pipelines than I'd like to admit, and it was time well spent.

**Feature engineering that mirrors business concepts.** Raw events don't mean anything to a salesperson. "User fired event `feature_x_activated` 14 times in the last 30 days" is not actionable. "User is a power user of the collaboration features" is actionable. Translating events into meaningful features requires understanding the product deeply — which features signal genuine engagement, which are noisy, which are correlated with conversion — and encoding that understanding into a pipeline that can run on fresh data.

**Presentation that changes behavior.** A dashboard that requires sales reps to learn a new mental model will not be used. A tool that fits into how sales reps already think about their pipeline — and makes the right insight visible without asking them to dig — will actually influence their decisions. The frontend work on these products is not glamorous, but it's consequential. I've seen technically sophisticated products fail because the UX required too much interpretation, and simple products succeed because the interface was obvious.

**Freshness matters.** A signal that tells you an account is heating up is only useful if you have it in time to act. This pushes toward lower-latency data pipelines, which push toward architectural tradeoffs I spent a lot of time navigating. Batch processing is simpler and cheaper; streaming is faster and more complex. The right answer depends on how time-sensitive the signals are, which depends on the sales motion, which you learn by watching salespeople actually use the product.

## What I Learned About B2B Product Engineering

B2B software is a specific discipline. The users are not the buyers. The people who evaluate whether to purchase your product (economic buyers, typically executives) are often not the people who use it day-to-day. This creates interesting product design constraints: you have to make the product good enough that users adopt it organically, while making the outcomes visible enough that buyers can justify the expense.

That duality shows up in engineering decisions. Audit logs, usage reports, admin dashboards, SSO and SCIM support — these features matter to buyers and are often invisible to users. They're not the fun features to build, but they're frequently the features that close enterprise deals. Learning to prioritize them is part of becoming a complete B2B engineer.

I also learned that **schema design is destiny in B2B SaaS**. The data model you commit to in the first six months will shape what you can build in the next three years. Getting it wrong is expensive — not immediately, but incrementally, as every new feature has to fight against a model that wasn't designed to accommodate it. The temptation is to design for the near term because the near term is urgent. The discipline is to hold enough of the long term in your head to avoid closing off important doors.

## On Working With Sales and Growth Teams

Software engineers and salespeople think differently. Sales teams operate on intuition, relationship, and urgency. Engineers operate on evidence, abstraction, and completeness. Both are valid. Both are limiting in excess.

The most valuable thing I learned working on sales-adjacent tooling was how to sit with ambiguity about whether something "worked." Engineers want metrics. Sales teams want wins. The conversion from one to the other is lossy and often delayed. A feature that improves the quality of conversations doesn't show up in a clean A/B test. A workflow that saves thirty minutes per rep per week doesn't announce itself.

Getting comfortable with that fuzziness — and learning to identify leading indicators that pointed toward value even when the lagging indicators weren't there yet — made me a better product engineer. Not just for B2B. For everything.

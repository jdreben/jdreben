---
title: "A Decade at the Frontier: What Startup Engineering Actually Teaches You"
date: 2025-10-15
description: "Ten years of building software at companies small enough that the decisions you make on Tuesday ship on Wednesday. Here's what that does to a person."
tags: ["engineering", "career", "startups"]
---

There's a specific kind of clarity you get when the production database is down at 11pm and you're the only engineer awake. No ticket to open, no escalation path. You diagnose it, you fix it, you write the post-mortem. And then you learn things that are genuinely hard to learn any other way.

I've spent the better part of ten years in that situation. Several startups across pretty different domains: urban mobility, revenue intelligence, remote work, AI products. One thing runs through all of them. Generalism under pressure.

## The First Question Is Rarely Technical

The hardest engineering problem isn't optimizing a hot path or picking a schema. It's knowing what to build first. Early-stage companies have almost no information and very little time, and you're choosing between ten things that all feel equally urgent with incomplete data.

What I figured out, slowly, is that "what do we build first?" almost always has the same answer: the thing that teaches you the most. Not the most impressive thing. Not the thing that makes the demo look slick. The thing that proves or kills your biggest assumption as fast as possible. Ship it, see what happens, adjust.

At one point I was at a startup with a hypothesis that sales teams were flying blind. They had CRM data but almost zero signal from how people actually used the product. The first version we shipped was embarrassingly simple. It didn't look like much. But it confirmed the hypothesis in a week. That's all that mattered. The sophisticated version came later, once we knew it was worth building at all.

## The Full-Stack Tax

At startups you own things you've never owned before. You write Python data pipelines on Monday and debug CSS layout problems on Thursday. You instrument your own monitoring because there's nobody else to do it. You're on-call for systems you built, which is either motivating or terrifying depending on how much you trust yourself. Honestly, both, sometimes.

I've written TypeScript APIs, Python ML pipelines, data visualization dashboards, CI/CD configuration, and SQL that would embarrass me now. I've shipped features in languages I hadn't touched before the sprint started. The breadth is uncomfortable but it compresses a lot of exposure into a short amount of time.

Across the stack, over ten years, roughly:

- **Infrastructure and reliability.** On-call rotations, observability pipelines, error budgets, postmortems. Treating reliability as a product feature, not a department.
- **Backend systems.** API design, database modeling, async job queues, event-driven architectures.
- **Frontend and product.** SvelteKit, React, Hugo, data visualization, enough CSS to be dangerous.
- **AI and machine learning.** Notebook-level experimentation through to production ML systems that needed to run reliably and cheaply at scale.

## Leadership Before You Feel Ready

Startups hand you responsibility before you feel ready for it. That's partly because there's nobody else, and partly because the founders at good companies are pretty good at figuring out who can handle more than their job description says.

Going from IC to tech lead to eventually CTO taught me that the hardest parts of software leadership aren't technical. It's knowing when to delegate the thing you'd rather do yourself. How to give feedback that actually changes behavior. How to make architectural calls when the right answer genuinely isn't clear, because it usually isn't.

I've made wrong calls. Some of them were expensive. The expensive ones taught me more.

## What I'd Tell Myself Earlier

Own the whole system. Even if you're hired as a backend engineer, learn enough about the frontend and the infrastructure to have a real opinion on it. Engineers who can reason across the stack make better decisions in every single layer.

Reliability is a product feature. Not an ops concern. If your product is unreliable, users don't trust it, and users who don't trust your product don't come back.

Simple code compounds. The clever solution saves two hours now and costs six hours of debugging six months from now. That trade almost never makes sense.

Write more. The engineers I've admired most write well. Not just docs, but proposals and post-mortems and actual thinking on paper. Writing is thinking, and thinking clearly is most of the job.

Ten years in, I'm still figuring things out. That's sort of the point.

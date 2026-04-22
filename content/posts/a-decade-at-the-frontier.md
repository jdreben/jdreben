---
title: "A Decade at the Frontier: What Startup Engineering Actually Teaches You"
date: 2025-10-15
description: "Ten years of building software at companies small enough that the decisions you make on Tuesday ship on Wednesday. Here's what that does to a person."
tags: ["engineering", "career", "startups"]
---

There's a particular kind of clarity that comes with small teams and real stakes. When a startup's production database is down at 11pm and you're the only engineer awake, you don't have the luxury of opening a Jira ticket. You diagnose, you fix, you write the post-mortem. You learn things that are genuinely hard to learn any other way.

I've spent the better part of a decade in that mode. A handful of startups across wildly different domains — urban mobility, revenue intelligence, remote work, AI applications, media — and one thread runs through all of them: the necessity of generalism under pressure.

## The 0-to-1 Problem

The hardest engineering problem isn't optimizing a hot path or designing a schema. It's knowing what to build *first*. Early-stage companies have almost no information and almost no time. You're choosing from among ten things that all feel equally urgent, with incomplete data and a runway that could be measured in months.

What I learned is that the answer to "what do we build first?" is almost always "the thing that will teach us the most." Not the most impressive thing. Not the thing that makes the demo look good. The thing that falsifies your biggest assumption or validates your riskiest bet. Ship it, watch what happens, learn, repeat.

At a revenue intelligence startup I joined, we had a hypothesis that sales teams were flying blind — they had CRM data, but no signal from actual product behavior. The first version of the product was almost embarrassingly simple. It wasn't impressive engineering. But it confirmed the hypothesis in a week, which is all that mattered. The sophisticated version came later, once we knew it was worth building.

## The Full-Stack Tax

At startups, you pay the full-stack tax. You own things you've never owned before. You write Python data pipelines on Monday and debug CSS layout issues on Thursday. You instrument your own monitoring because there's nobody else to do it. You're on call for systems you built, which is either motivating or terrifying depending on how much you trust yourself.

I've written TypeScript APIs, Python ML pipelines, data visualization dashboards, CI/CD configurations, Kubernetes YAML, and SQL that would embarrass me now. I've shipped features in languages I'd never used before the sprint started. The breadth is sometimes uncomfortable, but it compresses years of exposure into months.

The companies I've worked at have spanned pretty much every point on the stack:

- **Infrastructure and reliability** — owning on-call rotations, building observability pipelines, setting error budgets, doing postmortems. Treating reliability as a feature, not a department.
- **Backend systems** — API design, database modeling, asynchronous job queues, event-driven architectures.
- **Frontend and product** — React, SvelteKit, Hugo, data visualization, dashboard design, enough CSS to be dangerous.
- **AI and machine learning** — from notebook-level experimentation through to production ML systems that needed to run reliably, cheaply, and at scale.

## Leadership Without a Title

One thing startups do early is hand you responsibility before you feel ready for it. That's partly because there's nobody else, and partly because the founders who build good companies are usually good at spotting people who can handle more than their job description.

The progression from IC to technical lead to, eventually, CTO at my most recent startup taught me that the hardest part of software leadership isn't technical. It's knowing when to delegate what you'd rather do yourself, how to give feedback that actually changes behavior, how to hire people better than you in the specific areas that matter most right now, and how to make architectural decisions when the right answer genuinely isn't clear.

I've made wrong calls. Some of them were expensive. The expensive ones taught me more than the cheap ones.

## What I'd Tell Myself at the Start

**Own the whole system.** Even if your job title says "backend engineer," learn enough about the frontend, the infrastructure, the data, and the product to have an opinion on all of it. The engineers who can reason across the whole stack make better decisions in every layer.

**Reliability is a product feature.** Not an ops concern, not a DevOps ticket. If your product is unreliable, users don't trust it, and users who don't trust your product don't convert. Build observability in from the start.

**Simplicity compounds.** The clever solution that saves two hours of implementation costs six hours of debugging six months from now. Simple code, honest interfaces, conservative choices — they compound into a codebase that's a pleasure to work in rather than a tax you pay with every feature.

**Write more.** The engineers I've most admired all write well. Not just documentation — proposals, post-mortems, explorations of tricky problems. Writing is thinking, and thinking clearly is the job.

A decade in, I'm still learning. That's the point.

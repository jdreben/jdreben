---
title: "The 3am Rule: Site Reliability Engineering Without a Dedicated Team"
date: 2025-06-10
description: "Most startups can't afford a dedicated SRE team. Here's what I've learned about building reliable systems when reliability is everyone's responsibility."
tags: ["SRE", "infrastructure", "engineering"]
---

Most of the companies I've worked at couldn't afford a dedicated site reliability team. At the startup stage — and sometimes well past it — reliability is just another responsibility engineers carry alongside their feature work. You own what you ship. If it breaks at 3am, you wake up.

That constraint turned out to be one of the most educational of my career.

When you can't outsource reliability to a specialist team, you develop a different relationship with the systems you build. You think about failure modes before you ship because you'll be the one handling them. You instrument thoroughly because you'll need to debug in the dark. You build conservatively because you've already paid the tax on the clever solution that stopped making sense six months later.

Here are the principles that have held up across every environment where I've had to think seriously about reliability.

## Observability Before Alerts

The most common reliability mistake I see is investing in alerting before investing in observability. Alert on CPU, memory, error rate. Watch dashboards for anomalies. This is intuitive, and it's the wrong order.

The problem is that alerts tell you *something is wrong*. Observability tells you *what is wrong and why*. Without good observability, every incident becomes a debugging session with limited information. You're reading tea leaves from logs that weren't designed to answer the question you're asking.

The three signals worth instrumenting deeply — before you write your first alert — are:

**Structured logging.** Not just log strings, but log events with consistent fields: request ID, user context, operation name, duration, outcome. The discipline to emit structured logs at every meaningful point pays dividends when you're trying to trace a bad request through six services at 2am.

**Distributed traces.** If your system has more than one service — and eventually it will — traces let you follow a request through the whole system. Latency problems are almost always in the trace; without traces, you're guessing.

**Business-level metrics.** HTTP error rates tell you your service is sick. The metric that tells you *how sick* your business is — successful transactions per minute, feature utilization, conversion rates — is the one that anchors the conversation between an engineer saying "we're at 99.2%" and a product manager asking "but are users being affected?"

## Error Budgets as a Forcing Function

I first encountered error budget thinking through Google's SRE book, but it took several years of production incidents before the concept clicked for me intuitively.

The idea is simple: define a reliability target (say, 99.9% availability, which is roughly 43 minutes of downtime per month). The *gap* between 100% and your target is your error budget. You can spend it however you want — risky deploys, experiments, maintenance windows — but when you've spent it, you stop and fix reliability before doing anything else.

The reason this framework works is that it turns reliability from a vague aspiration into a concrete resource. "We should be more careful with deploys" is unanswerable. "We've burned 80% of our monthly error budget and it's the 12th" is a fact that changes behavior.

At one company, implementing error budgets meant that engineering finally had a legitimate reason to slow down feature delivery when reliability was degraded. The feature team wanted a new deployment. The error budget was in the red. The answer was no, and it was no for a data-driven reason that didn't require a difficult conversation about engineering culture.

## Post-Mortems That Actually Work

I've participated in a lot of post-mortems. Many of them were theater: a group of people in a room writing down what happened, assigning action items that were never tracked, and moving on. A few of them were genuinely useful.

The difference comes down to two things: **blamelessness** and **specificity**.

Blamelessness doesn't mean accountability-free. It means the post-mortem process is designed to improve systems, not punish people. Engineers make mistakes. The question worth answering is what in the system — the tooling, the process, the culture, the monitoring — allowed that mistake to cause an incident. Systems can be fixed. Shaming people just teaches them to hide problems.

Specificity means actionable action items. "Improve monitoring" is not an action item. "Add an alert on the orders-per-minute metric with a 15-minute window, firing if it drops 30% from the trailing 7-day median" is an action item. The difference between them is whether the post-mortem was useful or whether it was ceremony.

## On-Call as a Design Tool

Here's the thing about on-call that took me a while to fully internalize: if being on-call is miserable, that's information about your system design, not your engineers' pain tolerance.

An on-call rotation that wakes people up several times a week with noisy, low-signal alerts is a system design problem. It means alerts are miscalibrated, or the system has too much churn in metrics that don't actually reflect user impact, or the thresholds were set by guessing rather than by observing the system's normal behavior.

The engineers closest to the system are also the engineers best positioned to fix this. The practice of treating your own on-call experience as a signal — and actually acting on it — is what separates teams with sustainable reliability cultures from teams who are just grinding through.

The 3am rule I try to apply to everything I ship: *would I be comfortable waking up at 3am to deal with this?* If the answer is no, I haven't shipped it in a reliable enough state. That constraint focuses attention remarkably well.

## Reliability as a Feature

The last thing worth saying is that reliability isn't a tax. Users who trust that your product works return. Users who've experienced two or three moments of "what happened to my data?" stop returning. The compounding effect of reliability on product trust is real, and it's large, and it rarely gets modeled explicitly in product planning.

Reliability is a feature. It's unglamorous and it doesn't go in the release notes, but it's the feature that holds everything else up.

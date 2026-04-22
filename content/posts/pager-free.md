---
title: "The 3am Rule: Site Reliability Engineering Without a Dedicated Team"
date: 2025-06-10
description: "Most startups can't afford a dedicated SRE team. Here's what I've learned about building reliable systems when reliability is everyone's responsibility."
tags: ["SRE", "infrastructure", "engineering"]
---

Most of the companies I've worked at couldn't afford a dedicated site reliability team. Reliability is just another thing engineers carry alongside their feature work. You own what you ship. If it breaks at 3am, you wake up for it.

That constraint taught me more about reliability than any book has.

When you can't hand the problem off, you build a different relationship with the systems you write. You think about failure modes before shipping because you'll be the one dealing with them. You instrument more carefully because you know you'll be debugging in the dark at some point. You build more conservatively because you've already paid the tax on the clever solution that stopped making sense six months later.

Here's what has actually held up across every environment where I've had to think seriously about this stuff.

## Observability First, Then Alerts

The most common reliability mistake I see is building alerts before building observability. CPU, memory, error rate. Watch dashboards for anomalies. This is intuitive and its the wrong order.

Alerts tell you something is wrong. Observability tells you what is wrong and why. Without it, every incident turns into a debugging session with bad information. You're reading tea leaves.

Three things worth getting right before you write your first alert:

**Structured logging.** Not just log strings but log events with consistent fields: request ID, user context, operation name, duration, outcome. The payoff is being able to trace a bad request through several services at 2am without guessing at what happened.

**Distributed traces.** Once you have more than one service (and you will eventually), traces let you follow a request through the whole system. Latency problems are almost always visible in the trace. Without traces you're guessing.

**Business-level metrics.** HTTP error rates tell you the service is sick. What tells you how sick the business is, successful transactions per minute, feature utilization, conversion rate, is the metric that actually anchors real conversations between engineering and product. Build those.

## Error Budgets Work Because They're Concrete

I first read about error budget thinking in the Google SRE book. It took a few real production incidents before the concept actually clicked for me.

The idea: define a reliability target (99.9% availability is roughly 43 minutes of downtime a month). The gap between 100% and your target is your budget. Spend it however you want: risky deploys, experiments, maintenance windows. When its gone, stop and fix reliability before doing anything else.

This works because it turns reliability from a vague aspiration into a concrete resource. "We should be more careful with deployments" is unanswerable. "We've burned 80% of our monthly error budget and it's the 12th" is a fact that changes behavior.

At one company, error budgets gave engineering a legitimate reason to pump the brakes on feature delivery when reliability was degraded. The feature team wanted a new deploy. The budget was in the red. The answer was no, for a data-driven reason, without a hard conversation about engineering culture. That's the kind of structural protection that's hard to get any other way.

## Post-Mortems That Don't Suck

I've sat through a lot of post-mortems. Most of them were theater: write down what happened, assign action items nobody tracks, move on. A few were actually useful.

The difference comes down to blamelessness and specificity.

Blamelessness doesn't mean no accountability. It means the process is designed to fix systems, not punish people. Engineers make mistakes. What's worth asking is what in the system allowed that mistake to become an incident. Systems can be fixed. Shaming people just teaches them to hide problems next time.

Specificity means action items that are actually actionable. "Improve monitoring" is not an action item. "Add an alert on orders-per-minute with a 15-minute window, firing if it drops 30% from the trailing 7-day median" is an action item. The gap between those two things is whether the post-mortem accomplished anything.

## On-Call as a Design Signal

If being on-call is miserable, that's information about your system, not your pain tolerance.

A rotation that wakes people up multiple times a week with low-signal alerts is a system design problem. Alerts are miscalibrated, or the system has too much churn in metrics that don't reflect real user impact, or the thresholds were set by guessing rather than observing actual behavior.

The engineers closest to the system are the ones best positioned to fix this. Treating your own on-call experience as a signal and doing something about it is what separates teams with real reliability cultures from teams that are just grinding through.

The rule I try to apply to everything I ship: would I be comfortable waking up at 3am to deal with this? If the answer is no, its not done yet. That question focuses attention pretty effectively.

Reliability doesn't go in the release notes. But its what holds everything else up.

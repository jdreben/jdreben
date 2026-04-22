---
title: "Predicting Where People Go: What Urban Mobility Taught Me About Machine Learning"
date: 2025-08-22
description: "Seven years of building ML systems that forecasted human movement through cities — from Jupyter notebooks to production pipelines to a startup acquisition."
tags: ["machine learning", "AI", "startups", "zoba"]
---

In 2017, a friend and I started building software to answer a deceptively simple question: *where will people want to go next?*

Not in a theoretical sense. In a practical, operationally consequential sense: given that you have a fleet of electric scooters or bicycles distributed across a city, and given that demand shifts throughout the day, throughout the week, and with the weather — where should those vehicles *be* right now to serve as many trips as possible?

That question was the seed of [Zoba](https://craft.co/zoba), a machine learning company I co-founded and eventually led as CTO before its acquisition by [Marti](https://marti.tech) in 2024.

## The Problem Is Harder Than It Looks

The naive solution is obvious: look at where trips started in the past, and put vehicles there. The problem with the naive solution is that it's circular. If you always put vehicles where trips started before, you never learn about demand in places you haven't served. Your data reflects your deployment decisions, not the underlying demand.

This is a common and underappreciated problem in ML applied to the physical world. The data you collect is shaped by decisions your system already made. Build a model naively on that data, and you've built a feedback loop that ossifies your existing strategy rather than improving it.

The approach that worked better involved:

**Separating supply signal from demand signal.** Where a trip *started* is a function of where a vehicle *was*, which is a function of your deployment decisions. Where a trip *ended* is cleaner signal — it's roughly where people wanted to go. So you model both, and use them differently.

**Treating the city as a graph.** Cities aren't grids. They have structure — neighborhoods, transit hubs, topography, land use patterns. Representing the city as a spatial graph rather than a uniform discretization let us capture that structure in the model.

**Contextual features that actually move.** Time of day is obvious. Day of week is obvious. Weather is less obvious but significant — rain is a scooter suppressor. Local events (concerts, games, conferences) are huge but hard to source systematically. We learned that the signal in event data was worth the effort of wrangling it.

## From Notebooks to Production

When I talk to people early in their ML careers, I notice a gap between how they think about ML and what ML in production actually looks like.

The notebook phase — exploratory data analysis, feature engineering, model selection, offline evaluation — is the part that gets taught and celebrated. It's intellectually satisfying. You iterate fast, you visualize everything, and it's clear when you're making progress.

Production ML is different. In production:

- The model has to run on a schedule and be auditable when something goes wrong
- Feature pipelines have to be reliable, because a model fed bad features makes bad predictions silently
- You need monitoring for data drift, not just prediction accuracy
- The business logic that interprets and acts on predictions has to be correct — a good model plus bad business logic produces bad outcomes
- You own the whole thing at 2am if it breaks

The discipline I learned shipping ML systems at Zoba was to treat the feature pipeline with the same rigor as the model itself. More rigor, actually. A model is a function of its features. If the features are wrong, the model is wrong, and you may not know it.

## Growing Into the CTO Role

I became CTO not because I was the most technical person on the team — I wasn't, always — but because I was most willing to think about what we needed to build versus what was interesting to build. Those two things overlap more than people think, but not completely, and knowing the difference is most of the job.

What the technical leadership role actually meant at Zoba:

- **Hiring and developing engineers** who were smarter than me in specific domains, and creating an environment where they could do their best work
- **Making architectural decisions** about things like how to store spatial data, how to version features, when to use an external ML platform versus build in-house — with incomplete information and real tradeoffs
- **Managing technical debt as a resource**, not a failure. We accumulated debt deliberately and paid it down deliberately. The mistake isn't taking on debt; it's taking it on invisibly.
- **Interfacing with the business side** — making the ML legible to non-technical stakeholders, building trust in the model's recommendations, explaining why the model was sometimes wrong

## What Comes After an Acquisition

When Marti acquired Zoba in early 2024, I felt two things simultaneously: pride and relief. Pride because we'd built something valuable enough that a sophisticated company wanted it. Relief because acquisitions are exits, and exits mean the thing you built mattered.

The ML systems we built powered real decisions about real vehicles in real cities. Thousands of scooter trips happened where they happened partly because of predictions our models made. That's the kind of concrete impact that makes the 2am pages worth it.

The actual science of human movement — how people decide to travel, how cities shape those decisions, how infrastructure either affords or forecloses certain trips — remains genuinely fascinating to me. If I ever start something again, it'll probably be adjacent to this problem space.

Urban mobility taught me that the hardest thing in applied ML isn't the machine learning. It's understanding the system you're trying to model well enough to ask the right questions of the data.

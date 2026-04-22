---
title: "Predicting Where People Go: What Urban Mobility Taught Me About Machine Learning"
date: 2025-08-22
description: "Seven years of building ML systems that forecasted human movement through cities, from Jupyter notebooks to production pipelines to a startup acquisition."
tags: ["machine learning", "AI", "startups", "zoba"]
---

In 2017, a friend and I started trying to answer what seemed like a simple question: where will people want to go next?

Not philosophically. Practically. If you have a fleet of electric scooters distributed around a city, and demand shifts throughout the day and week and with the weather, where should those vehicles actually be right now to serve the most trips?

That question was the seed of [Zoba](https://craft.co/zoba), the company I co-founded and eventually led as CTO until its acquisition by [Marti](https://marti.tech) in early 2024.

## The Naive Approach Doesn't Work

The obvious solution is to look at where trips started historically and put vehicles there. The problem is that this is circular. If you always deploy to where trips started before, you never learn about demand anywhere you haven't already served. Your data reflects your own decisions, not the underlying demand.

This is a subtle and genuinely underappreciated problem in applied ML. The data you collect is shaped by decisions your system already made. Build a model naively on that data and you've built a feedback loop that locks in your existing strategy rather than improving it.

What worked better was a few things working together.

Separating supply signal from demand signal. Where a trip starts is a function of where a vehicle was, which is a function of your deployment choices. Where a trip ends is cleaner signal, its roughly where people actually wanted to go. You model both, and use them differently.

Treating the city as a graph instead of a uniform grid. Cities have structure. Neighborhoods, transit hubs, topography, land use. Representing all of that spatially let us capture things a naive grid approach would miss.

Contextual features that actually move. Time of day is obvious. Day of week is obvious. Weather matters more than you'd expect; rain basically kills scooter trips. Local events are huge but hard to source systematically. We spent a surprising amount of time on event data and it paid off.

## Notebooks vs. Production

There's a gap in how early-career ML folks think about modeling versus what shipping ML actually looks like.

The notebook phase: exploratory analysis, feature engineering, model selection, offline evaluation. That's the part that gets taught. It's satisfying. You iterate fast, you visualize everything, and its obvious when you're making progress.

Production ML is different. The model has to run on a schedule and be auditable when something breaks. Feature pipelines have to be reliable, because a model fed bad features makes bad predictions silently, and that's the worst kind of wrong. You need drift monitoring, not just accuracy metrics. And you own all of it when it falls over at 2am.

The discipline I built at Zoba was treating the feature pipeline with at least as much rigor as the model. A model is a function of its features. If the features are wrong, the model is wrong, and you might not know it for weeks.

## Becoming CTO

I didn't become CTO because I was the most technical person on the team. I wasn't, always. I became it because I was the one most focused on what we needed to build versus what was interesting to build. Those things overlap more than people expect, but not completely, and the difference is a lot of the job.

In practice that meant: hiring people smarter than me in specific areas, making architectural decisions about things like spatial data storage and feature versioning with incomplete information, managing technical debt as a deliberate resource rather than a failure, and making the ML legible to people who had to trust it without building it.

That last one was harder than I expected.

## After the Acquisition

When Marti acquired us in early 2024 I felt proud and also relieved. Proud because we'd built something valuable enough that a serious company wanted it. Relieved because it meant the thing we built mattered.

The ML systems we shipped powered real decisions about real vehicles in real cities. Thousands of scooter trips happened where they happened partly because of predictions our models made. That's the kind of concrete impact that makes the 2am pages worth it.

Urban mobility as a domain is still genuinely fascinating to me. How people decide to move through space. How cities shape those decisions. How infrastructure either enables or forecloses certain trips. If I build something again, it'll probably live somewhere near this problem.

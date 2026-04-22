---
title: "What Building Remote Work Software Taught Me About Distributed Systems"
date: 2024-12-18
description: "On the technical and human challenges of building software that makes distributed teams feel less distributed."
tags: ["engineering", "distributed systems", "infrastructure"]
---

Remote work tooling is a deceptively interesting engineering domain. On the surface, you're building collaboration software: video, messaging, shared documents, virtual spaces. Underneath, you're solving a cluster of genuinely hard distributed systems problems, made harder by the fact that your users have strong intuitions about what "presence" and "togetherness" feel like — and those intuitions are unforgiving when the software falls short.

I spent a period of my career building infrastructure for a company trying to make distributed teams feel less distributed. Here's what that taught me.

## Presence Is a Hard Problem

When you're in a physical office, you know who's around. You notice when someone walks by. You pick up on ambient cues — keyboard sounds, side conversations, the general hum of other people working. Recreating this in software is surprisingly difficult.

The naive approach is status indicators: green dot means online, grey means away. But status indicators are a lie. They're updated on a schedule, or manually, or based on proxy signals like "last heartbeat received." They don't reflect what people are actually doing. Everyone who has used Slack knows that a green dot tells you almost nothing about whether someone is available.

The more interesting approach is ambient presence — software that gives you a lightweight, always-on sense of what your colleagues are up to, without requiring explicit status updates. This is technically hard for a few reasons:

**Real-time updates at scale are expensive.** Presence information has to be fresh. A 30-second staleness window is often unacceptable. But pushing updates to every client in an organization every few seconds is a significant infrastructure load, and the naive implementation doesn't scale. The production solution involved a combination of event-driven updates for meaningful state changes, periodic polling for low-stakes presence signals, and careful connection management to minimize server-side overhead.

**Network conditions are adversarial.** Your users have laptops on home WiFi, mobile hotspots, corporate VPNs, and hotel connections. The software has to work acceptably across all of them. Graceful degradation — ensuring that core features work even when real-time features can't — turns out to be a significant design discipline. You don't notice it when it works. You definitely notice it when it doesn't.

**Perceived latency is not actual latency.** Users don't experience raw milliseconds; they experience the *feeling* of responsiveness. Optimistic updates (reflecting a user's action in the UI before the server confirms it) can make software feel fast even when network round-trips are slow. Getting this right, especially in the face of server errors that require rolling back the optimistic state, is finicky but worth it.

## Async-First Is an Architecture, Not a Policy

One thing that makes remote work tools interesting is that they have to serve two modes simultaneously: synchronous interaction (video calls, live chat, real-time collaboration) and asynchronous interaction (comments, status updates, recorded demos, long-form writing).

Companies that do remote work well typically have strong async cultures. They write things down. They communicate in ways that don't require the recipient to be present. They record decisions. This is partly cultural, but software can either support or undermine it.

The architectural implication is that async-first features need to be *at least as good* as their synchronous equivalents. If your async video message tool is harder to use than hopping on a Zoom call, people will hop on Zoom calls. The default wins.

Building features that were genuinely competitive with synchronous alternatives taught me to think carefully about **friction as a design variable**. The goal isn't zero friction — some friction is meaningful (a confirmation dialog before a destructive action) — but friction should always be intentional, always justified, and always the minimum necessary for safety or clarity.

## WebRTC Is a Minefield

No piece of software infrastructure I've worked with has a higher ratio of apparent simplicity to actual complexity than WebRTC.

In theory: browsers can establish peer-to-peer audio and video connections. In practice: NAT traversal, STUN/TURN server management, codec negotiation, simulcast, bandwidth estimation, connection renegotiation, and a long list of browser-specific behaviors make video infrastructure genuinely hard to get right. We used managed WebRTC infrastructure (rather than building it from scratch), which was the right call — but we still spent significant engineering time on edge cases, reliability, and quality of experience.

The lesson I took from this is that the infrastructure layer you *don't* see is often the hardest part of the product to build. Users experience video quality and connection reliability directly, and they're ruthless evaluators. A video call that drops twice a week is a product that doesn't work. There's no forgiveness in the latency budget.

## Software Shouldn't Try to Simulate the Office

The honest realization I reached toward the end of this work is that the best remote work software doesn't try to replicate the office. It does something better: it takes what's genuinely *good* about distributed work — flexibility, focus time, diverse geography, asynchronous communication — and makes it easy to do those things well, rather than apologizing for not having a conference room.

The products I found most effective treated remote work as a different thing from office work, not an inferior substitute. They optimized for async-first workflows, made written communication feel lightweight rather than laborious, and supported real-time collaboration where it added genuine value rather than everywhere.

That design philosophy had engineering implications too. Building for what distributed work actually is, rather than trying to simulate what it isn't, led to more honest technical tradeoffs and, ultimately, better software.

The engineers who built that software — including, in some small part, me — were thinking about a real problem. How people work together across distance, across timezones, across contexts. That problem isn't going away.

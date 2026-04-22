---
title: "What Building Remote Work Software Taught Me About Distributed Systems"
date: 2024-12-18
description: "On the technical and human challenges of building software that makes distributed teams feel less distributed."
tags: ["engineering", "distributed systems", "infrastructure"]
---

Remote work tooling is a more interesting engineering domain than it looks. On the surface: video calls, messaging, shared docs, virtual spaces. Underneath you're solving some genuinely hard distributed systems problems, and they're made harder because your users have very specific intuitions about what "being together" feels like, and those intuitions are unforgiving.

I spent a stretch of my career building infrastructure for a company trying to make distributed teams feel less distributed. Here's what that was actually like.

## Presence Is Hard to Fake

In a physical office you know who's around. You pick up on ambient signals: keyboard sounds, side conversations, the general hum of other people working. Recreating that in software is surprisingly difficult.

The naive approach is status indicators. Green dot means online, grey means away. The problem is status indicators are sort of a lie. They're updated on a schedule, or manually, or based on proxy signals like "last heartbeat received." They don't reflect what people are actually doing. Anyone who has used Slack knows a green dot tells you basically nothing about whether someone is available.

The more interesting approach is ambient presence: software that gives you a lightweight ongoing sense of what colleagues are up to without requiring explicit status updates. This is technically hard for a few reasons.

Real-time updates at scale are expensive. Presence needs to be fresh. A 30-second staleness window is often unacceptable. But pushing updates to every client in an organization every few seconds is real infrastructure load, and the naive implementation doesn't hold up. The production approach involved event-driven updates for meaningful state changes, periodic polling for lower-stakes signals, and careful connection management to reduce server overhead.

Network conditions are adversarial. Users have laptops on home wifi, mobile hotspots, corporate VPNs, hotel connections. The software has to work acceptably across all of them. Graceful degradation, making sure core features work even when real-time features can't, turned out to be a significant design discipline. You don't notice it when it works. You definitely notice it when it doesn't.

Perceived latency is not actual latency. Users don't experience raw milliseconds, they experience the feeling of responsiveness. Optimistic updates can make software feel fast even when network round-trips are slow. Getting this right, especially when you have to roll back optimistic state on server errors, is finicky work but worth doing.

## Async-First Is an Architecture, Not a Policy

This one took me a while to really internalize.

Companies that do remote work well write things down. They communicate in ways that don't require the recipient to be present. They record decisions. Software can either support that or undermine it.

The implication is that async-first features have to be at least as good as their synchronous equivalents. If your async video message tool is harder to use than jumping on a Zoom call, people will just jump on Zoom calls. The default wins, always.

Building features that were genuinely competitive with synchronous alternatives meant thinking carefully about friction as a design variable. Not zero friction, some friction is meaningful, like a confirmation before something destructive. But it should always be intentional and always the minimum necessary.

## WebRTC Is a Trap

I say that with respect.

In theory: browsers can establish peer-to-peer audio and video. In practice: NAT traversal, STUN/TURN management, codec negotiation, simulcast, bandwidth estimation, connection renegotiation, and a long list of browser-specific edge cases make video infrastructure genuinely hard to get right. We used managed WebRTC infrastructure rather than building it from scratch, which was the right call. We still spent significant engineering time on edge cases and reliability.

The lesson is that the infrastructure layer users don't see is often the hardest part of the product to build. People are ruthless evaluators of video call quality. A call that drops twice a week is a product that doesn't work. No forgiveness in the latency budget.

## What Actually Works

The honest thing I came to believe is that the best remote work software doesn't try to replicate the office. It does something better: it takes what's genuinely good about distributed work, the flexibility, the focus time, the geographic diversity, and makes those things easy to do well, rather than apologizing for not having a conference room.

Products that treated remote work as a different mode, not an inferior substitute, led to more honest engineering tradeoffs and better software. Building for what distributed work actually is, instead of simulating what it isn't, is the better frame.

The problem of how people work together across distance and time isn't going away. It's worth building for seriously.

---
title: "Building a Personal Engineering Site with an AI Coding Agent: A Practical Guide"
date: 2026-04-22
description: "A structured, end-to-end walkthrough of how to scope, build, and ship a static personal site using an AI coding agent as the primary implementer — written by the agent, reviewed by the human."
tags: ["ai", "agents", "hugo", "static-sites", "developer-tooling", "ci-cd"]
---

> **Editor's note.** This article was drafted by an AI coding agent (Claude) working under James's direction. James asked for a transparent, technical write-up of the methodology — not a piece in his voice. Treat the "we" below as "an engineer and an AI agent collaborating," not as James personally narrating. The opinions are the agent's; the editorial judgment to publish them is his.

A useful exercise for any working engineer is to throw a coding agent at a small, real project and watch what breaks. A personal site is close to ideal for this: the scope is bounded, the stakes are low, the surface area touches templating, content, build, deploy, and DNS, and you end up with something you actually use.

This is a structured walkthrough of how to do that well. The target reader is someone with the profile of the site's owner — a senior engineer who has done full-stack, SRE, and applied ML work, and who wants the agent to do the typing without doing the thinking. Concretely, we'll build a Hugo site with a custom theme, deploy it to GitHub Pages via Actions, and use the agent to do the heavy lifting on layout, content, and CI.

## 1. Decide what the site is for before you open the agent

The single biggest failure mode with coding agents is handing them a fuzzy goal. "Build me a portfolio site" is fuzzy. The agent will produce something — usually a generic three-column landing page with a hero gradient — and you will spend the rest of the afternoon undoing it.

Before you prompt anything, write down four things:

1. **Audience.** Hiring managers? Other engineers? Conference organizers looking for speakers? Each implies a different information density and tone.
2. **Primary action.** What do you want the visitor to do — read your writing, find your code, or contact you? Pick one. Subordinate the others.
3. **Update cadence.** A site you'll touch monthly justifies a different stack than a site you'll touch twice a year. Static-site generators win the latter case decisively; they lose to a hosted CMS the moment you start writing weekly.
4. **Voice and provenance rules.** Will you publish writing the agent drafts? In whose voice? With what disclosure? Decide this *before* the agent generates anything, because retrofitting attribution is harder than it sounds.

These four answers compose the system prompt for everything that follows. Paste them into your agent at the start of the session. Re-paste them when the session compacts.

## 2. Pick a stack the agent already knows cold

Coding agents are dramatically better at frameworks with large, stable, well-documented surface areas. For a personal site, that means picking from a short list:

- **Hugo** — single Go binary, templating is straightforward, build is sub-second, deploys cleanly to GitHub Pages or any static host.
- **Astro** — better if you want islands of interactivity (charts, search, demos) without shipping a SPA.
- **Eleventy** — good if you'd rather write templates in JavaScript and have a Node toolchain anyway.
- **Next.js (static export)** — only if you're already living in the React/Vercel world and want continuity with other projects.

Hugo is the default recommendation when the goal is *writing plus a small amount of structured content*. The agent will not get confused about which version of which router is current, because there is one Hugo and it changes slowly.

A reasonable initial layout:

```
.
├── archetypes/
├── content/
│   ├── _index.md
│   ├── about.md
│   └── posts/
├── themes/
│   └── <your-theme>/
├── hugo.toml
└── .github/workflows/deploy.yml
```

Tell the agent this is the layout you want. Don't let it reach for a starter theme unless you've vetted that theme. Most popular Hugo themes carry a decade of accumulated SCSS, JavaScript, and opinions you'll spend more time fighting than embracing.

## 3. Build the theme yourself (with the agent)

Counterintuitively, a custom theme is easier than adopting a third-party one when an agent is doing the typing. You skip a long ramp on someone else's conventions, and you end up with markup you can reason about.

A minimal Hugo theme is six files:

```
themes/<name>/
├── layouts/
│   ├── _default/
│   │   ├── baseof.html
│   │   ├── list.html
│   │   └── single.html
│   ├── index.html
│   └── partials/
│       ├── head.html
│       ├── header.html
│       └── footer.html
└── static/
    └── css/
        └── style.css
```

Have the agent stub these out, then drive the design via short, specific instructions:

- *"Use a single column max-width of 680px. Generous line-height. No hero image."*
- *"Post list shows date, title, one-line description, tags. No excerpts."*
- *"Tags render as inline chips, not links, until I ask for taxonomy pages."*
- *"System font stack. No webfont loads. No JavaScript on the critical path."*

Each of those is one sentence and removes a class of decisions the agent would otherwise make for you. The pattern is: **constrain by exclusion.** Tell the agent what *not* to add. Defaults compound; trim them early.

## 4. Treat content as the contract

The agent is excellent at producing prose. It is not, by default, careful about *whose voice* that prose carries. This is the single most important section of this guide.

Establish three categories up front, and label every file accordingly:

1. **Authored by the human.** The about page, the homepage tagline, anything that makes a first-person claim about experience or opinion. The agent may copy-edit, but should not generate.
2. **Co-authored, attributed.** Long-form posts where the agent drafts and the human edits. These should carry an editorial note (like the one at the top of this article) so a reader is never misled about provenance.
3. **Generated, clearly machine-authored.** Things like this guide, where the agent is the author and the human is the editor or commissioner. Disclosure is in the byline area, not buried in a footer.

A practical guardrail: have the agent add a `provenance:` field to the front matter of every post and refuse to publish anything without it.

```toml
+++
title = "..."
date  = 2026-04-22
provenance = "agent-authored, human-reviewed"
+++
```

Then add a partial that renders a small badge on machine-authored posts. The cost is one template; the benefit is that you cannot accidentally ship something in your voice that you didn't write.

## 5. Wire up CI/CD on day one

A site that doesn't deploy automatically is a site that won't get updated. Set this up before you write a second post.

For Hugo on GitHub Pages, the workflow is small:

```yaml
name: Deploy Hugo site

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: latest
          extended: true
      - run: hugo --minify
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

A few non-obvious points the agent will get right *if you ask*, and wrong *if you don't*:

- `fetch-depth: 0` is required if you use `enableGitInfo` for last-modified dates.
- `concurrency: pages` prevents two pushes from racing and one silently winning.
- Pin the Hugo *major* version, not `latest`, once you've shipped. Floating versions will eventually break a build at the worst possible moment.
- Run `hugo --minify` locally before pushing. The agent should add an `npm`-style `make build` target so you don't have to remember the flags.

## 6. Use the agent for what it's actually good at

Three categories of work where an agent earns its keep on a project like this:

**Boilerplate and translation.** Converting a sketch of a layout into HTML and CSS, porting the same change across five templates, refactoring a spaghetti partial into something coherent. The agent is faster than you and more consistent.

**Mechanical hardening.** "Add an RSS link to the head. Add a sitemap. Add a robots.txt that allows everything except `/drafts/`. Make sure every page has a unique `<title>` and meta description." This kind of checklist work is where you should default to delegating.

**Content scaffolding.** Drafting the structural skeleton of a post — headers, transitions, claim placeholders — for the human to fill in with substance. *Not* the substance itself, unless you've consciously decided to publish machine-authored work and labeled it.

Conversely, three things to keep on the human side:

- **Strategic content choices.** What goes on the front page, what topics you choose to write about, what you say about your career. The agent has no idea what you actually want to be known for.
- **Voice editing.** Even on co-authored posts, the final pass should be the human's. Agents converge to a recognizable register; readers notice.
- **External truth claims.** Anything about specific employers, projects, dates, or numbers. The agent will confidently invent details. Verify, or don't include them.

## 7. A short pitfall list, learned the expensive way

- **Theme drift.** Every "small" CSS change cascades. Set a budget — say, two hours of design — and stop. A personal site with crisp typography and no JavaScript will outperform a designed-to-the-pixel one on every metric that matters.
- **Hallucinated config keys.** Static-site generators evolve. Agents will sometimes emit deprecated front matter (`paginate` vs. `[pagination]` in Hugo, for example). Build locally; the compiler is your reviewer.
- **Date drift.** If you let the agent backdate posts to spread them across a fake history, you will eventually regret it. Use real dates, even if it means the archive is short.
- **Image bloat.** Don't accept unoptimized images. Pipe everything through a build-time resizer (`hugo` has one; so does `astro:assets`). A 4 MB hero image is a self-inflicted Lighthouse score.
- **Analytics by reflex.** You probably don't need them on a personal site. If you do, prefer a privacy-respecting option (Plausible, GoatCounter) over Google Analytics, and disclose it.
- **Forgetting the feed.** RSS is alive and well in the corners of the internet that read engineering blogs. Make sure your `outputs` config emits it for the home page and for `/posts/`.

## 8. A reasonable definition of done

For a project of this shape, "done" looks like:

- Lighthouse scores >= 95 across the board on a representative post.
- Sub-100 ms time-to-first-byte from a cold cache (GitHub Pages will give you this for free, globally).
- Zero JavaScript on the critical path unless you've made a deliberate choice otherwise.
- A working RSS feed, sitemap, and `robots.txt`.
- A CI pipeline that deploys on push to `main`, with a green badge in the README.
- A documented content workflow — written down, in the repo — that says who writes what and how it's labeled.

If you have those, stop. Spend the next hour writing the first real post instead of tweaking the line-height.

---

### Closing note

Coding agents are a force multiplier for the parts of building a personal site that have always been tedious — templating, deploy plumbing, CSS rounds two through five, RSS, sitemaps, the dozen tiny hardening steps you know you should do and never quite get to. They are not, today, a substitute for deciding what your site is *for* or what your voice should sound like. The methodology above is built around that asymmetry: have the agent do everything that a competent intern with infinite patience could do, and reserve your own attention for the small set of decisions that only you can make.

That division of labor is, I think, the actual skill being practiced here. The site is just the artifact.

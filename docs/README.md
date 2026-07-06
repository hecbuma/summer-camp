# Neta Summer Program

> A hands-on Rails apprenticeship: a setup week to get your machine and your git habits ready, then 6–8 weeks of real work on a real codebase, ending with something you build yourself.

---

## How this works

This is not a course. There are no lectures and no slides. You learn the way working engineers actually learn: you get assigned real work, you figure out what you need to know to do it, and understanding follows from doing.

A few principles that run through the whole thing:

**Tasks first, concepts second.** You won't read three chapters before touching code. You'll run `rails db:migrate` on day one, watch something happen, and *then* understand what a migration is. The work pulls the learning behind it.

**Figure it out yourself first.** When you hit something you don't understand, your first move is to investigate — read the code, read the docs, experiment in the console. If you're still stuck after a real attempt, bring it to the weekly sync. "I don't know what this does" is a fine thing to say *after* you've tried.

**Stuck for more than a day is a sync topic.** Don't burn three days spinning on one problem. There's a line between productive struggle and wasted time, and a day is roughly it.

**The concept map is yours.** Each phase lists concepts you should be able to explain by the end of it. It's not a test schedule — it's a self-check. If you finish a phase and can't explain one of them, that's a gap worth closing before you move on. Your coach will follow up, but the responsibility is yours.

**LLMs are allowed everywhere.** Use Claude, Cursor, whatever you want, for everything. There are no restrictions. But understand the deal: **you will be judged on whether you can explain and defend what you shipped, not on whether you typed it yourself.** An LLM can write the code. It can't understand it for you. When your coach asks "walk me through this method," the LLM won't be there.

**PR reviews are real.** Expect inline comments, change requests, and pushback. That is not criticism — that is the job. Learning to take a code review well is one of the most valuable things you'll get out of this.

---

## The setup

You'll each **fork the Neta repo** and work in your own copy. You're not sharing a codebase. You work through the same backlog independently, at your own pace, making your own decisions.

This means:

- You never block each other. No merge conflicts, no waiting.
- Your codebases will diverge as you make different choices. That's expected and good.
- You'll **compare solutions** at checkpoints. After a meaty ticket, the three of you look at both forks side by side. "Why did you use a callback here when you didn't?" Those conversations teach more than the ticket itself.
- Peer review is informal: read each other's PRs, leave comments, ask questions. Your coach gives the real review on each fork.

---

## The arc

| Stage | When | What you're doing |
|---|---|---|
| **Week 0 — Setup** | Before you start | Get a working machine, learn the git loop, get the lay of the land. |
| **Phase 0 — Foundation** | Week 1 | Get the app running. Set it up for AI-assisted work. First real PRs. |
| **Phase 1 — Core product** | Weeks 2–4 | Ship real features end to end. Auth, dashboards, feedback. |
| **Phase 2 — Prompt engineering** | Weeks 5–6 | Treat the LLM scoring as an engineering problem. Measure and improve it. |
| **Phase 3 — Build your own** | Weeks 7–8 | Leave Neta behind. Build a v0 from scratch using it as your reference. |

Week 0 is a runway, not a phase — it's the work you do to be *ready* to start. Once the program proper begins at Phase 0, every phase is valuable on its own, so if things run short you stop at a natural boundary.

---

## Week 0 — Setup & Fundamentals

A runway week. Before you can hit the ground running, you need ground to run on: a working development machine, enough git to live inside the pull-request workflow this whole program uses, and a rough map of what "backend" means. None of this is graded — it's the price of admission to week one.

The work is still task-first. You're not reading about git, you're *doing* a real pull request. You're not studying environments, you're standing one up.

> **Your full step-by-step guide is the companion handout, _Week 0 — Setup & Fundamentals_.** It walks through every command for Windows. This section is the overview; that handout is what you actually follow.

### What you'll do

- **Stand up a real dev environment.** On Windows, that means Linux running inside Windows (WSL2 + Ubuntu), then Ruby, Node, PostgreSQL, and VS Code wired together. All your real work happens inside Linux — the same environment Rails runs on in production.
- **Get connected to GitHub** with an SSH key so you can push code.
- **Get Neta running locally** at `localhost:3000` on your own machine.
- **Copy the project board to your own account.** Every ticket lives on a GitHub Project board. You'll make your own copy so your progress stays separate from the other student's, then convert a ticket into a real issue in your fork each time you start one.
- **Do the whole git loop once, for real** — branch, change, commit, push, open a pull request, get it reviewed and merged. Zero stakes, just the muscle memory.
- **Read the backend primer** so the vocabulary of week one isn't a wall of unfamiliar words.

### Concepts you should be able to explain by the end of this week

- What the command line is, and the handful of commands you'll use daily (`cd`, `ls`, `mkdir`, running a script)
- What git is *for* — why branches and commits exist, what a pull request actually is
- The difference between frontend and backend, in your own words
- What the request/response cycle is
- What a database is and why apps use one instead of files
- What it means to run a server locally, and what `localhost:3000` is

### You're ready for week one when

- [ ] Linux runs and you can open a terminal in it
- [ ] Ruby, Node, and PostgreSQL are installed and working
- [ ] VS Code opens connected to your Linux environment
- [ ] Your SSH key is on GitHub and authentication works
- [ ] Neta runs at `localhost:3000`
- [ ] You've opened and merged your first pull request
- [ ] You've read the backend primer

Anything you can't get past after a genuine attempt — bring it to the first sync. Setup snags are the one place where asking early is the right call, not a last resort.

---



### Tickets

**NETA-001 · Write CLAUDE.md**
Document the project so an AI agent can work in it effectively. Architecture summary, conventions, how to run tests, the patterns the codebase uses, what's off-limits. Then *test it*: open a fresh Claude Code session with only this file as context and try to get real work done. Fix whatever's missing. You'll use and refine this file for the rest of the program.

**NETA-002 · Audit and fix the intake form**
Use the app like a real user. Find what's off — validation gaps, confusing labels, missing error states, weird edge cases on location input. Fix at least two things. Open your first PR with a clear description of what was wrong and why.

**NETA-003 · Seed data for development**
The app has no seed data. Build a realistic seed file: venues across a range of craft/hype scores, recommendations in different modes, feedback in various states. Every ticket after this will lean on it.

### Concepts you should be able to explain by the end of this phase

- What a migration is and what it does to the database
- MVC, and where each layer lives in this codebase
- What a route is and how a URL becomes code that runs
- What the Rails console is and why it's useful
- What a gem is and where dependencies come from
- What a service object is and why this app uses them instead of fat controllers
- What makes a `CLAUDE.md` actually useful versus noise

---

## Phase 1 — Core product `weeks 2–4`

This is where Neta moves from a personal experiment toward something real users could touch. You'll work through the backlog below. Tickets within a horizon can be done in any order, but the sequence is roughly the priority.

### Backlog

**NETA-010 · Magic link authentication**
User model, secure token generation, email delivery, session persistence. No passwords — user enters email, gets a link, clicks it, they're in. Token expires after use. Integrate cleanly with the existing session-based identity so a logged-in user's recommendations associate with their account.
*Touches: model, mailer, controller, routes, sessions*

**NETA-011 · User recommendation history**
Once authenticated, show users their past recommendations and outcomes — date, venues, ranks, whether they marked satisfied. Useful immediately, and it sets up personalization later.
*Depends on: NETA-010*

**NETA-012 · Admin dashboard — recommendations**
Internal view of all recommendations across sessions. Sortable by date, mode, score. Shows craft score, hype score, inflation index, and feedback outcome per venue. This is the interface that tells you whether the core hypothesis is working.
*Touches: query optimization, scopes, Turbo for filtering*

**NETA-013 · Admin dashboard — scoring health**
Extend the admin to flag venues that might be mis-scored — high inflation index but satisfied feedback, or low inflation but unsatisfied. These anomalies are exactly what Phase 2 will dig into.
*Depends on: NETA-012*

**NETA-014 · Feedback enrichment**
Feedback is currently binary. Add a regret level (1–5) and an optional free-text note. Keep the primary action one tap — the extra fields are optional and secondary. Update the admin views to show the richer data.
*Touches: migration, model validation, UI, existing feedback flow*

**NETA-015 · Improved loading experience**
The recommendation flow has real API latency. Add per-step progress using Turbo Streams — "Finding venues…", "Scoring craft…", "Ranking results…". The user should feel progress, not a blank wait.
*Touches: Turbo Streams, controller, service layer*

### Concepts you should be able to explain by the end of this phase

- What ActiveRecord is and how it maps to database tables
- Model associations, and how you model relationships between things
- What `has_secure_token` does and why it fits magic link auth
- Callbacks: when they help and when they're a trap
- What Turbo is and why this app has almost no custom JavaScript
- What makes a test valuable versus a test that just passes
- What a scope is and when to use one over a plain `where`
- N+1 queries: what they are and how you'd spot one in the dashboard

---

## Phase 2 — Prompt engineering is engineering `weeks 5–6`

The scoring prompt is the core IP of Neta — and it's an open hypothesis. It might be wrong. This phase treats the LLM not as a magic box but as a component you measure, test, and improve like any other.

### Backlog

**NETA-020 · Prompt audit and variant testing**
Using the admin dashboard, find 3–5 venues where the scores feel wrong. Document why. Write three prompt variants, each with a written hypothesis *before* you test it. Run all variants against the same venues. Compare outputs systematically. Write up your findings and open a PR with changes if the data justifies them.
*Output: a writeup plus a PR*

**NETA-021 · Provider swap experiment**
Swap the scoring adapter from Gemini to Anthropic for a set of test venues. Compare outputs side by side — score distribution, consistency, cost. Recommend which to use and defend it.
*Touches: the LlmAdapter pattern, environment config, testing*

**NETA-022 · Scoring confidence score**
Add a confidence measure to scoring — how much review data the model actually had. Few reviews means low confidence. Surface it in the UI and admin, and factor it into ranking: a low-confidence hidden gem should rank below a high-confidence one.
*Touches: prompt, scorer, model, UI*

### Concepts you should be able to explain by the end of this phase

- Why wording and structure in a prompt change the output meaningfully
- What it means to *evaluate* an LLM — what's your ground truth?
- Why the adapter pattern exists, and what a provider swap actually costs
- What temperature is and how it affects scoring consistency
- The difference between a prompt that works once and one that works reliably

---

## Phase 3 — Build your own v0 `weeks 7–8`

Leave Neta behind. Start from zero. Neta is now your **reference implementation** — look at it any time, but every decision in your new app is yours to make and yours to defend.

### The constraint

You are not given an idea. You are given a shape:

> **Build a tool that solves a personal pain point, uses at least one external API, and scores or ranks something using an LLM — with a feedback loop so the user can tell the app whether the output was useful.**

The interesting work is in *what* you choose to score, and *why*. Find your own domain.

### Tickets

**OWN-001 · Pitch it**
One paragraph: what you're building and what pain it solves. Get it approved before you write a line of code.

**OWN-002 · CLAUDE.md first**
Write your `CLAUDE.md` before you write the app. Set the project up for AI-assisted development from day one — you've now seen what a good one looks like.

**OWN-003 · Design the domain model on paper**
Entities, relationships, the key decisions. Before any code. One page.

**OWN-004 · `rails new` and justify it**
Every generator and config decision is yours. Be ready to explain each one — why this database, why these defaults, what you turned off.

**OWN-005 · Ship one complete flow**
Intake → external API → LLM scoring → ranked result → feedback. End to end, with tests. Not polished, not complete. One flow that works.

**OWN-006 · Demo and defend**
Final session: demo it, then walk through every architectural decision you made and what you'd do differently. This is the real exam, and it's a conversation, not a quiz.

### Concepts you should be able to explain by the end of this phase

- What decisions `rails new` actually makes for you, and why
- How you design a domain model before you have any code to lean on
- How you integrate an external API you've never used before
- The minimum a v0 needs to actually test its hypothesis
- What it would take to make your app production-ready

---

## Stretch backlog

Real tickets, not busywork. Pick these up on Neta if you move through a horizon faster than expected.

**NETA-040 · Background job for stale venue rescoring** — venues older than 7 days rescore automatically. Requires Sidekiq + Redis. A real architectural addition: queues, error handling, retries.

**NETA-041 · Location autocomplete** — replace the plain text location input with Google Maps autocomplete. Better UX, more precise coordinates.

**NETA-042 · Regret risk score** — implement the fourth scoring dimension: weighted by wait time, price tier, effort, and hunger mode. Surface in UI, factor into ranking.

**NETA-043 · Planning mode refinements** — planning mode currently ranks by craft only. Filter for reservable venues, weight uniqueness, allow higher price tiers. Needs Places API fields not currently fetched.

**NETA-044 · Email digest** — weekly email showing a user's recommendation history and accuracy rate. How often did Neta get it right? Mailer + background job + a stats query.

---


## For the coach

Notes for running Lane 0, kept separate from the learner-facing material above.

**Protect momentum above all.** This learner is the most fragile to early discouragement. Celebrate the small wins loudly and genuinely — a working guessing game is a real milestone at this stage. The fastest way to lose them is a stretch of unexplained red errors, so stay close early.

**Higher-touch than the other lanes.** Expect shorter, more frequent check-ins than the once-a-week rhythm the others get, and be ready for a bit of the light pairing you were hoping to avoid — especially around modules 6 and 7, where you're the one starting Neta and guiding the first change. That hands-on presence is the point here, not a failure of the model.

**LLMs off, then on — and mean it.** The "turn the AI off" instruction in the learner intro is the single most important rule in this lane. A beginner with an LLM produces working code they understand zero percent of, and *feels* like they learned. Hold the line through module 5 at least. Reintroduce AI tools deliberately once they can read and reason about code on their own — and when you do, make the deal explicit: it's for building on understanding, not replacing it.

**No fixed endpoint.** This lane is self-paced and doesn't have to reach any particular module by summer's end. Getting solidly through, say, module 4 with real understanding beats rushing to module 7 with none. Let the pace be theirs.

**They're a minor.** GitHub accounts are allowed at 13+, so 15 is fine, but make sure a parent or guardian knows they're in the program and using GitHub. Keep everything they touch age-appropriate and public-facing-safe.

**The merge point.** Module 7 is where Lane 0 rejoins the main road — the scavenger hunt and first tiny PR are deliberately the same shape as Phase 0 in the main program, just with far more support. When they're comfortable there, they're ready to start picking up real backlog tickets, slowly, with review.

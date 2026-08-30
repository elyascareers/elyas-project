# Applied AI: Directing Agents to Build Software & Automate Content

**By Elyas Zulqarnain** &middot; Portfolio project #3 (AI)

*What this project shows: I don't just "use AI" to answer questions &mdash; I direct AI
agents to research, build, test, ship, and run real products, under rules that keep the
output safe and honest. This is a portfolio of that applied, agentic work.*

---

## 1. What "AI fluency" means here

Most people mean "I use ChatGPT." I mean something more hands-on: I run **agentic
workflows** &mdash; giving AI agents a goal, a set of operating rules, and the tools
(code, browser, connectors, schedules) to carry a job from start to finish, with me as the
director and reviewer. The evidence is real products that were researched, built, tested,
and shipped this way, plus a content operation that runs itself on a schedule.

I work under **Zetranova Limited**, an AI-enabled digital commerce & business-services
studio (Accra, Ghana). Everything below was built there.

---

## 2. Agentic product development &mdash; a repeatable, governed pipeline

Rather than build each product from scratch, I standardised a **Digital Product Builder**
pipeline that takes any product from a one-page intake to a delivered, tested build:

> **Intake &rarr; Research &rarr; Scaffold &rarr; Build &rarr; Verify (live) &rarr; Handoff**

What makes it more than "ask AI to code" is the **governance** I hold it to &mdash; the
same discipline a serious engineering team would demand:

- **Trust boundary:** anything fetched from the web (competitor sites, docs) is treated as
  *data, not instructions*. No code, links, or package names cross that boundary &mdash; a
  direct defence against prompt-injection and "slopsquatting" (fake package names).
- **No fabrication:** never invent testimonials, reviews, stats, team members, or client
  logos. Empty beats fake &mdash; fabricated reviews are a real legal exposure under the
  FTC's rule, which explicitly covers AI-generated ones. Missing values ship as visible
  placeholders, never as convincing fakes.
- **Secrets & supply chain:** credentials never enter chat; `.env` is git-ignored from the
  first commit; every dependency is verified to actually exist; the diff is scanned for
  secrets before any push.
- **Human-in-the-loop gates:** the agent proceeds freely through research, building, and
  local testing, but **stops for my explicit yes** before creating a repo, pushing,
  deploying, spending money, touching a live site, or sending anything to a client.
- **A real quality gate:** nothing is called "done" until the build passes, lint passes,
  `npm audit` runs, every page is exercised live in a browser, and it meets **WCAG 2.2 AA**
  (keyboard, contrast, labels). On one build, the contrast sweep caught a genuine
  accessibility failure that was then fixed.
- **Self-correction limits:** a maximum of 3 fix attempts on any failing build, and
  *weakening a check is never a fix* (no silencing type errors, no skipping tests). A red
  build reported honestly beats a green build that lies.

---

## 3. Products built and shipped this way (owned &mdash; Zetranova)

| Product | What it is | Notable engineering |
|---|---|---|
| **StockSafe** | Offline inventory app for Ghanaian shops &mdash; "stock that can't be cheated" | Installable PWA, works offline on a cheap Android; English + Twi; append-only records + auto theft flags; PIN roles; 56 logic tests pass |
| **SafeRent** | Escrow-protected rental marketplace | Flutter app + NestJS API + PostgreSQL/PostGIS; Ghana Card identity + property verification; MoMo escrow on a **double-entry, idempotent ledger**; SMS-OTP 2FA; mapped to OWASP MASVS-L2 |
| **Zetranova Finance Portal** | Accounting / inventory / payroll / HR portal | Real **double-entry ledger** behind plain-language screens; financial ratios, budget-vs-actual, 30/60/90-day cash-flow forecast; honestly benchmarked against QuickBooks / Xero / Wave / Zoho |
| **SocialManager** | Social-media scheduler (Metricool-style) | Next.js; background **worker with retries/backoff**; per-network publish outcomes; a **demo/real safety interlock** so sample posts can never reach a live audience; 53 automated assertions + a time-travel simulation |
| **Stitchbook** | Order/measurement app for tailors | Zero-dependency Node; scrypt password hashing, CSRF, unguessable tracking links; WhatsApp "is it ready?" links; PWA |
| **School Management System** | Per-school management system for Ghanaian basic schools | Ghana GES/NaCCA/SBA defaults (all editable); Creche vs Standard report cards; finance, attendance, inventory; swappable data layer (localStorage or PHP+PDO API); tenant-aware |
| **Zetranova landing** | Corporate landing site for the studio | Static, accessible (skip links, ARIA, reduced-motion), SEO with JSON-LD, mobile-first |

*(Also in the suite: Indigo web app, ZetClass, and a school landing page.)*

The through-line: every one ships with **honest limits documented**, a security posture,
accessibility, and a "what still needs a real-world value" list &mdash; not a demo that
looks good until you touch it.

---

## 4. Agentic content automation &mdash; a pipeline that runs itself

Beyond building, I automate my content brand (**Simple Titbit**) with a set of **scheduled
AI agents** that research, draft, and publish on a weekly cadence &mdash; with a human
approval gate in the middle so nothing goes out unreviewed:

| When | Agent | What it does |
|---|---|---|
| **Mon 10:03** | Blogging agent | Researches and drafts the week's 3 site posts + a Substack digest + social spokes, logs them, and **stops at a human approval gate** (ready 24h before first publish) |
| **Tue 10:00** | Website + Substack publisher | Publishes the approved posts and sends the Substack digest |
| **Tue & Fri 12:00** | Social publisher | Publishes approved social content (X, TikTok, Instagram, etc.) at the best-practice window |
| **Tue/Fri, hourly 9&ndash;6** | X login guardrail + retry | Warns before noon if X isn't logged in, then retries the pending post the moment login is detected |

This is genuine orchestration: multiple agents, best-practice timing, a **human-in-the-loop
gate**, and failure/retry **guardrails** &mdash; not a single prompt. It also produced
published ebooks, including one titled *AI Made Simple*.

---

## 5. My AI competency, mapped

| Capability | Evidence |
|---|---|
| **Agentic building** | The Digital Product Builder pipeline + the products in &sect;3 |
| **AI governance / safety** | Documented operating rules: trust boundary, anti-fabrication (FTC-aware), secrets & supply-chain checks, quality gate |
| **Prompt engineering** | A prompt-engineering booklet and reusable master build-prompts I authored |
| **Custom Claude skills** | I created reusable skills &mdash; e.g. research, "clear explanation", booklet layout, a product-development skill, competitive UX benchmarking, and a publication content skill |
| **Connectors / tools (MCP)** | I work across GitHub, Render, Google Drive/Calendar/Gmail, Figma, WordPress, and social/analytics tools through connected integrations |
| **Agentic scheduling** | The 4-agent Simple Titbit pipeline above, with HITL gates and retry logic |
| **Human-in-the-loop discipline** | Approval gates before anything irreversible &mdash; publish, deploy, spend, or send |

---

## 6. Why this matters for an employer

I can take a fuzzy goal and turn it into a shipped, tested product or a running automated
workflow &mdash; and I do it with the guardrails that keep AI output safe, honest, and
accessible. That combination &mdash; **hands-on AI fluency plus judgment about when to stop
and ask** &mdash; is the practical, hireable version of "AI skills."

*Prepared as portfolio project #3 (AI). The companion guide in `Documentation` explains how
to set up this way of working yourself.*

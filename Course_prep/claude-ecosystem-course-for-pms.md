# The Claude Ecosystem for PMs
### Intensive Workshop Curriculum

**Format:** 4 live classes · 60 minutes each
**Level:** Intermediate PM — no engineering background required
**Labs:** 7 hands-on exercises using participants' real work
**Total lab time:** ~110 minutes across the 4 classes

---

## Who It's For

- Senior PMs at AI-adjacent companies who use Claude daily but suspect they're leaving most of its capability untapped
- Directors deploying Claude across a team who need a framework for Skills, Plugins, MCP connectors, and guardrails
- PMs who want to write specs agents can execute, not just prompts
- Founders/operators who want research, synthesis, and reporting workflows running autonomously

**Not for:** people wanting a prompt library, a beginner's intro to LLMs, or a certificate with no real deliverables. Every lab runs on the participant's actual work — their PRDs, their data, their product space.

**Promise:** participants leave with a real Claude deployment designed for their team, not a slide deck about AI potential.

---

## Curriculum Overview

Each class builds on the last: from understanding the terrain, to mastering the building blocks, to running autonomous long-horizon agents, to deploying across an organization.

| # | Class | Focus | Lab time |
|---|-------|-------|----------|
| 1 | The Terrain | Claude.ai surfaces, models, mental models | 20 min |
| 2 | The Building Blocks | MCP, Skills, agentic primitives | 30 min |
| 3 | The Engine Room | Claude Code, long-running agents, channels | 35 min |
| 4 | The Full Deployment | Cowork, Computer Use, enterprise architecture | 25 min |

---

## Class 1 · The Terrain
**The Claude Platform — Mental Models, Models & the Claude.ai Surface**

**What you'll learn**
- Anthropic's platform stack layers and why they matter for PM strategy
- Every Claude.ai surface: Projects, Memory, Artifacts, Connectors, plan tiers
- Model tiers (Haiku / Sonnet / Opus) and the intelligence-vs-cost decision framework
- Extended Thinking vs. Multi-Agent — the two levers for scaling AI intelligence
- Research-style multi-agent systems as a live, observable pattern

**Lab 1A · PM Workspace Setup (20 min)**
Set up your actual PM workspace in Claude: real Projects, real instructions, real documents. Leave with a configured environment, not a demo.

---

## Class 2 · The Building Blocks
**MCP, Skills & the Agentic Primitives**

**What you'll learn**
- MCP as protocol, not product — the USB-C analogy and why an open standard matters
- Web vs. desktop vs. custom connectors — where each lives and what it unlocks
- Skills: how context-efficient skill loading works and when to build one
- The PMSP decision matrix: Prompt vs. Project vs. Skill vs. MCP
- Slash commands as structured PM workflows that chain Skills together

**Lab 2A · Build Your PM Skill (15 min)**
Use a skill-creation workflow to scaffold a Skill from your own methodology — a PRD formatter, a discovery interview guide, or a competitive brief template.

**Lab 2B · Agentic Chain (15 min)**
Connect 3 MCP servers (e.g. Slack, Drive, web) and trigger a task that crosses systems. Debrief: where did Claude need course-correction?

---

## Class 3 · The Engine Room
**Claude Code, Long-Running Agents & Channels**

**What you'll learn**
- Claude Code architecture: the agentic loop, CLAUDE.md, multi-context window design
- Common long-running-agent failure modes and how to design around them
- The initializer/coding-agent harness pattern, and why it's useful beyond engineering
- Channels: the shift from pull to push — Telegram, Discord, iMessage as agent interfaces
- What channel architecture choices reveal about enterprise deployment strategy

**Lab 3A · First Claude Code Task (15 min)**
Run a real Claude Code task at one of three difficulty tiers — research synthesis, document processing, or spec-to-prototype. Pick based on comfort level.

**Lab 3B · Autonomous Research Loop (20 min)**
Run a full autonomous research loop on your own product space. Write the CLAUDE.md that would let tomorrow's session pick up where today's left off.

---

## Class 4 · The Full Deployment
**Cowork, Computer Use & Enterprise Architecture**

**What you'll learn**
- What Cowork's sandboxed environment adds beyond desktop chat
- Persistent agent threads: mobile-to-desktop continuity
- Computer Use: the priority stack, risk framework, and PM judgment model
- Claude for Excel and PowerPoint — the Cowork → Excel → Deck workflow
- Enterprise essentials: private plugin marketplaces, admin controls, analytics, self-serve rollout

**Lab 4A · Full Cowork Task (15 min)**
Complete a real PM deliverable — synthesis brief, stakeholder deck outline, or exec summary — using Cowork's full capability. Compare output quality to your manual process.

**Lab 4B · Capstone — Design Your Team's Claude Stack (10 min, or extend to a group workshop)**
Design the real Claude deployment for your team or company: surfaces, Projects, Skills, MCP connectors, Plugins, guardrails, and rollout plan. Leave with an actionable blueprint.

---

## Full Lab List

| Lab | Class | Deliverable |
|-----|-------|-------------|
| 1A — PM Workspace Setup | 1 | Configured Claude workspace with real Projects, instructions, and an Artifact from real research |
| 2A — Build Your PM Skill | 2 | A working Skill built from your own methodology |
| 2B — Agentic Chain | 2 | A cross-system task run through 3 connected MCP servers |
| 3A — First Claude Code Task | 3 | A completed Claude Code task at your chosen difficulty tier |
| 3B — Autonomous Research Loop | 3 | A competitive research loop + a working CLAUDE.md |
| 4A — Full Cowork Task | 4 | A completed real PM deliverable produced end-to-end in Cowork |
| 4B — Capstone: Team Stack Design | 4 | A blueprint for your team's Claude deployment |

---

## Included Materials

- Project instruction templates for common PM domains
- Skill scaffolding templates (PRD, discovery, competitive brief)
- PMSP decision framework reference card
- Team Stack Design worksheet for enterprise deployment
- Recordings of all 4 sessions

---

## Suggested Logistics

- **Session length:** 60 min live + async lab completion time
- **Cadence:** 2 classes per week over 2 weeks (mirrors the reference workshop's Mon/Tue pattern)
- **Group size:** small cohort recommended so Lab 3B and 4B get facilitator feedback
- **Prerequisite:** regular use of an LLM; no engineering background required

---

## How to Actually Build and Run This

**1. Build content per class, not all at once**
For each class, produce three things before you touch slides: the teaching outline (what you say), the lab instructions (what participants do), and the reference artifact (template/worksheet they keep). Build Class 1's three pieces fully before starting Class 2 — the labs depend on tools participants set up in earlier classes.

**2. Pilot with 3–5 real PMs first**
Run Class 1 live with a small trusted group before opening registration. The labs are the whole value proposition here — if Lab 1A's "configure a real workspace" instructions are too vague, you'll find out in 5 minutes with a pilot group instead of in a paid cohort.

**3. Tooling you need lined up**
- A video platform with breakout capability (Zoom, Google Meet) for lab time
- A shared doc/Notion space for templates and worksheets participants can copy
- Claude accounts with Pro/Max access for every participant (Cowork, Claude Code, and MCP connectors all require paid tiers)
- A place to host recordings post-session (the "lifetime access" promise in the reference model)

**4. De-risk the labs**
Labs 2B (MCP chain) and 3A/3B (Claude Code) are the most likely to break in a live session — connector auth, install issues, API limits. Write a fallback path for each ("if MCP won't connect, use this pre-recorded walkthrough + a sandbox account") so one participant's broken setup doesn't stall the room.

**5. Registration and pricing**
The reference workshop charges $997 one-time for 4 classes + recordings + templates + community access. If you're building this for external sale, that's a reasonable anchor for a PM-specific intensive; if it's for an internal team, drop the pricing section and frame Lab 4B's capstone as the actual deliverable leadership sees.

**6. Sequencing check before you launch**
Confirm each lab is achievable inside its allotted time by timing yourself doing it cold. Reference workshop labs run 15–20 min each — that's tight for Lab 3B (autonomous research loop) and Lab 4B (capstone), so plan for those to spill into async completion with a follow-up review, not strict in-class finish.

**7. Iterate after each cohort**
Keep a running list of where participants got stuck (usually connector setup and Claude Code first-run). Fix the instructions, not just the individual — that's what turns this from a one-time delivery into a repeatable course.

---

## Reference Videos (Jyothi Nookula / Learn AI With Jyothi)

Channel: [youtube.com/@LearnAIWithJyothi](https://www.youtube.com/@LearnAIWithJyothi) — 13+ years leading AI product at Netflix, Meta, AWS, and Etsy; publishes weekly on practical Claude/AI workflows.

Note: YouTube's channel page is JavaScript-rendered, so I couldn't pull a complete, current video list programmatically. The titles below are confirmed hits from her channel and related interviews — browse the channel directly (link above) for her full, up-to-date catalog before finalizing which videos to pair with each class.

**Directly relevant to this curriculum**
- [The Claude Setup That Let a PM Beat 30 Engineering Teams](https://www.youtube.com/watch?v=uEK9ONplfRk) — pair with Class 1 or 3; she rebuilds the exact system that won an internal hackathon, good real-stakes framing for "spec quality > prompt tricks"
- [Stop Applying to AI PM Jobs Until You Watch This](https://www.youtube.com/watch?v=RlsOGvrpEsw) — good pre-work / context-setting video on where the PM role is heading with agentic tools

**Interviews covering the same material (audio/written, useful prep)**
- [The complete Claude stack for PMs, with Jyothi Nookula](https://www.aakashg.com/jyothi-nookula-claude-stack-podcast/) — Product Growth podcast; walks through Projects, Skills, MCP, Claude Code, Cowork, Computer Use and when to use each (maps closely to Class 2 & 4)
- [The complete Claude stack for PMs (Substack writeup)](https://www.news.aakashg.com/p/the-complete-claude-stack-for-pms)
- [AI PM at Netflix, Amazon and Meta — Here's How to Become an AI PM](https://www.news.aakashg.com/p/jyothi-nookula-podcast) — fundamentals + job search framing, good for the "who this is for" framing

**Her own platform (for cross-referencing scope/pricing/positioning)**
- [learn.jyothinookula.com](https://learn.jyothinookula.com/) — main course hub, including the workshop this curriculum is modeled on
- [AI Agents Masterclass for Product Managers (Maven)](https://maven.com/p/f543be/ai-agents-masterclass-for-product-managers) — adjacent masterclass, useful for comparing depth/format choices

**How to fill this out further:** open the channel link above, sort by "Newest," and pull the specific uploads tagged Claude Code, MCP, Skills, Cowork, and Computer Use — she publishes weekly, so a manual pass will surface more than search indexing does.

---

## Labs — Detailed Instructions

Full step-by-step instructions for all 7 labs (objectives, prerequisites, walkthroughs, deliverables, troubleshooting) are in the companion file: **`claude-ecosystem-course-labs.md`**

## Learning Resources

Official docs, engineering blog posts, and primary sources for every topic in the curriculum, organized by class, are in the companion file: **`claude-ecosystem-course-resources.md`**

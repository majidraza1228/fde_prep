# FDE & PM Interview Prep System

Structured preparation for **Forward Deployed Engineer (FDE)** and **Product Manager (AI)** interviews at AI-native companies — Anthropic, OpenAI, Palantir, Scale AI, Glean, Nvidia, and similar.

Built to be used with **Claude Code** (`claude` CLI). The slash commands turn Claude into a live interviewer, flashcard coach, and case study partner — all grounded in the study material in this repo.

**To share this with someone else:** send them this repo URL and point them to the Quick Start below. All they need is Node.js, an Anthropic API key, and 3 commands (`npm install`, `git clone`, `claude`). The slash commands work immediately with no additional setup.

---

## Quick Start (New User)

### Prerequisites

- **Node.js 18+** — check with `node --version`
- **Anthropic API key** — get one at [console.anthropic.com](https://console.anthropic.com)

### 1. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Then set your API key (one-time):

```bash
export ANTHROPIC_API_KEY=sk-ant-...
```

To make it permanent, add that line to your `~/.zshrc` or `~/.bashrc` and run `source ~/.zshrc`.

### 2. Clone this repo and open it

```bash
git clone https://github.com/majidraza1228/fde_prep.git
cd fde_prep
claude
```

### 3. Start practicing immediately

Type any slash command at the Claude Code prompt:

```
/fde
/pm
/drill
/case
/hedgefund
/assetmanagement
/concept RAG
```

That's it. Claude reads the study files in this repo and runs a live mock interview.

### How it works

When you run `claude` inside this folder, Claude Code automatically loads all files in `.claude/commands/` as slash commands. The commands tell Claude which role to play (interviewer, flashcard coach, skeptical customer) and which study files to draw from. You don't need to configure anything — it works as soon as you clone the repo and open it.

---

## Slash Commands — Full Reference

All commands live in `.claude/commands/`. Claude Code loads them automatically when you open this repo.

---

### `/fde` — FDE Mock Interview

Runs a realistic Forward Deployed Engineer interview. Picks a random area, asks one question at a time, scores every answer honestly.

**Areas covered:**
- Agent system design
- Customer scenario (on-site blocker, stakeholder pushback)
- Token optimization / cost engineering
- Security (prompt injection, CISO explanation, secrets)
- Observability and debugging
- Platform-specific (Claude API or OpenAI)

**Scoring format after every answer:**
```
What was strong: ...
Where you'd lose points: ...
What to study: ...
Score: Pass / Borderline / No hire
```

**Usage:**
```
/fde
```
Then just answer the question. Claude pushes back if you're vague.

---

### `/pm` — AI PM Mock Interview

Runs a Product Manager interview for AI-native companies (Anthropic, OpenAI, Glean). Tests technical depth AND product judgment — answers must include user/business value, not just technical definitions.

**Areas covered:**
- Product sense (design a feature, define a north star metric)
- Technical depth (transformers, RAG, hallucination, MCP, context engineering)
- Execution (launch plan, go-to-market, cross-functional tradeoff)
- Strategy (build vs buy, prioritization, make vs partner)
- Safety/ethics (when not to ship, responsible AI decisions)
- Conflicting metrics (one metric up, one down — what do you do?)

**Signature push:** After every technical answer, Claude asks:
> "Can you go one level deeper?"

**Usage:**
```
/pm
```

---

### `/drill` — Flashcard Drill

Rapid-fire flashcard session. One question at a time. Claude does not give the answer until you respond. Keeps score across 10 questions, surfaces your top gaps at the end.

**Available decks:**

| Deck | What it drills |
|---|---|
| `agent` | Agent loop code, stop_reasons, tool result format |
| `cost` | Pricing, caching math, model tiering, batch API |
| `security` | Prompt injection fixes, CISO explanation, secrets |
| `logs` | The 6 things to log on every API call |
| `debug` | 5 failure modes and how to detect each |
| `metrics` | North star, hallucination rate, refusal rate, eval pass rate |
| `concepts` | Transformers, RAG, fine-tuning, MCP, context engineering |
| `frameworks` | RICE, product sense structure, conflicting metrics answer |
| `safety` | When not to ship, ethics answer frame |

**Usage:**
```
/drill agent
/drill cost
/drill concepts
/drill           ← Claude picks a deck randomly
```

---

### `/case` — Cold Case Study

Claude plays a skeptical enterprise customer. Hands you a vague problem cold. Stays in character — no help, no hints — until you reach a clear recommendation with tradeoffs. Then breaks character and scores you.

**FDE scenarios:**
- Law firm: 50k docs, data can't leave VPC
- Hedge fund CTO: "just fine-tune our model on our data" (wrong instinct)
- IT team blocking API access on-site
- Customer workflow doesn't match the product
- Agent gave wrong answer in prod — explain it to the VP

**PM scenarios:**
- Design an AI feature (one-sentence brief only)
- North star metric for an enterprise copilot
- DAU up 20%, satisfaction down 15% — what do you do?
- Build RAG in-house vs buy a vendor?
- Safety team flagged a feature sales already promised

**Scoring checks:**
- Did you ask clarifying questions before proposing?
- Did you handle the customer's pushback?
- Did you give a recommendation with tradeoffs?
- Did you mention cost or risk?

**Usage:**
```
/case
```

---

### `/hedgefund` — Hedge Fund & Asset Management Mock

Full mock interview set entirely inside the hedge fund / asset management vertical. Covers both FDE and PM angles.

**FDE mode** — Claude plays three customer characters simultaneously:
- **CIO** — wants speed and analyst productivity, skeptical
- **Head of Compliance** — blocks on data residency and audit trail, will kill the project
- **Head of IT** — says little, controls all access, quietly blocks without approval

**FDE scenarios:**
1. Research synthesis agent over 50k internal documents
2. Regulatory filing automation (Form PF, Form ADV)
3. Client letter generation for 47 LPs — zero hallucination tolerance
4. Real-time portfolio monitoring with plain-English alerts
5. IT access blocker — project stalled, demo in 5 days

**PM mode — questions include:**
- Design a research copilot for fundamental analysts
- Should we support fine-tuning on proprietary fund data?
- Autonomous portfolio rebalancing — do you build it? (safety)
- Why is hallucination a bigger problem in finance than other industries?

**Scoring always checks three things specific to this vertical:**
1. Did you address data residency (nothing leaves the VPC)?
2. Did you propose an audit trail?
3. Did you require human-in-the-loop before anything ships?

**Usage:**
```
/hedgefund
```
Claude asks FDE or PM, then runs the scenario.

---

### `/assetmanagement` — Traditional Asset Management Mock

Full mock interview set inside the traditional asset management vertical — BlackRock, Vanguard, Fidelity, State Street, PIMCO, pension funds, endowments, and sovereign wealth funds. Covers both FDE and PM angles.

**Key distinction from `/hedgefund`:** Traditional AM is about managing trillions across millions of accounts at scale — not alpha generation. Compliance scrutiny on client communications is higher (FINRA supervision), procurement cycles are longer, and the core platform (Aladdin, Charles River) controls access to everything.

**FDE mode** — Claude plays three customer characters simultaneously:
- **Head of Distribution / CRO** — cares about advisor productivity and AUM retention, hates added friction
- **Chief Compliance Officer** — owns SEC/FINRA/DOL exposure, must supervise all client communications
- **Head of Technology / CTO** — controls Aladdin integration, long vendor security review cycles

**FDE scenarios:**
1. Wealth management at scale — AI-drafted advisor communications with compliance supervision
2. Fund research Q&A — natural language over 3,000+ proprietary reports, nothing leaves the network
3. Institutional client reporting — 600 pension funds, 6-week manual cycle, legal liability question
4. ESG regulatory audit — 230 funds, 8-week SEC filing deadline
5. Advisor onboarding copilot — confident wrong answers are the risk
6. Aladdin integration blocker — board demo in 3 weeks, IT says 10 more weeks

**PM mode — questions include:**
- Design an AI copilot for a financial advisor — what's your north star (not "time saved")?
- How does an AI company build something valuable without being blocked by Aladdin?
- Should we white-label our AI product under a large AM firm's brand?
- A CCO asks: how do we prove FINRA that a human reviewed every AI-drafted communication?

**Scoring always checks:**
1. Did you address the FINRA supervision requirement (not just data privacy)?
2. Did you propose a human approval workflow with an audit trail?
3. Did you acknowledge the platform integration constraint (Aladdin/Charles River)?

**Usage:**
```
/assetmanagement
```
Claude asks FDE or PM, then runs the scenario.

---

### `/concept` — Explain Any AI Concept at Interview Depth

Name any AI concept. Claude explains it at PM/FDE interview depth — not a textbook definition, not a research paper. Always ends with a ready-to-say interview answer and the follow-up question the interviewer will ask.

**Format for every concept:**
```
One-liner (memorize this)
One level deeper (2-3 sentences of how it works)
Why it matters — FDE angle
Why it matters — PM angle
Interview answer (say this out loud)
Follow-up the interviewer will ask — and the answer
```

After explaining, Claude offers to quiz you cold on the concept.

**Concepts covered:**
Transformers, attention mechanism, hallucination, RAG, fine-tuning, embeddings, vector database, context window, context engineering, temperature, tokens, inference vs training, orchestration, MCP, prompt caching, model tiering, batch API, VPC, private endpoints, prompt injection, tool use, agentic systems, evals, RLHF, system prompt, few-shot prompting, structured outputs, north star metric, RICE, LLM vs ML model, open source vs proprietary models — and anything else you ask.

**Usage:**
```
/concept RAG
/concept transformers
/concept MCP
/concept hallucination
```

---

## Study Files

| File | What's in it |
|---|---|
| `fde-mock-interview-session.md` | Master study guide — FDE + PM interview process, all technical areas, foundational concepts, frameworks, daily practice checklist |
| `asset-management-hedgefunds.md` | Hedge fund / asset management vertical — use cases, constraints, customer objections, architecture, FDE + PM scenarios |
| `system-design-agentic.md` | Agent system design deep dive — orchestration, failure modes, interview Q&A |
| `agent-loop-cheatsheet.md` | The complete `run_agent` function — write this from memory daily |
| `prompt-caching.md` | Prompt caching deep dive — rules, cost math, best practices |
| `microsoft-azure-github.md` | Azure OpenAI, Managed Identity, Content Safety, GitHub Actions |
| `study-plan.md` | Full 3-week study plan — day by day, time-boxed, with daily habits and progress tracker |
| `how-to-add-concepts.md` | Process for adding new concepts, verticals, and slash commands to the system |
| `reference-material.md` | Official docs, links, and industry vertical index |

---

## Recommended Study Plan

See [`study-plan.md`](study-plan.md) for the full day-by-day breakdown with time estimates, daily habits, and a progress tracker.

**Summary:**

| Week | Theme | Key commands |
|---|---|---|
| Week 1 | FDE technical foundations | `/drill agent` `/drill cost` `/drill security` `/fde` |
| Week 2 | PM layer on top | `/pm` `/concept` `/hedgefund` `/assetmanagement` |
| Week 3 | Cold mocks, gap closing | `/case` `/fde` `/pm` `/hedgefund` `/assetmanagement` |

**Daily habit (5 min every morning — write from memory):**
1. Full `run_agent` function with tool result format
2. The 6 things to log on every API call
3. One-liner: RAG vs fine-tuning
4. One-liner: prompt caching savings
5. Model tiering: Haiku → Sonnet → Opus
6. CISO prompt injection explanation in plain English
7. North star metric definition + one example

---

## How to Add a New Concept or Vertical

See [`how-to-add-concepts.md`](how-to-add-concepts.md) for the full step-by-step process.

**Quick version for a new concept:**
```
/concept [concept name]
```
Claude explains it at interview depth and offers to save it to the study file automatically.

**Quick version for a new vertical** (e.g. Legal, Healthcare):

1. Create a study file: `legal-lawfirms.md` — industry context, use cases, compliance constraints, objections
2. Create a command file: `.claude/commands/legal.md` — interviewer persona, scenarios, scoring rubric
3. Add a row to the Study Files table and a section to Slash Commands in this README
4. Add a `- [x]` entry to Areas Covered

Use `asset-management-hedgefunds.md` + `.claude/commands/assetmanagement.md` as a template — they're the most complete example of the pattern.

**Adding a new slash command in general:**

Any `.md` file you drop into `.claude/commands/` becomes a `/commandname` slash command automatically the next time you run `claude` in this repo. No registration or config needed.

---

## Areas Covered

- [x] System Design for Agentic Systems
- [x] Token Optimization and Cost Engineering
- [x] Security — Prompt Injection, Sandboxing, Secrets
- [x] Observability and Debugging
- [x] Platform: Claude API
- [x] Platform: OpenAI (Responses API, Structured Outputs, Batch API)
- [x] Platform: Microsoft / Azure / GitHub Copilot
- [x] Prompt Caching — Deep Dive
- [x] Foundational Concepts (Model vs Harness, RAG, Fine-tuning, Embeddings, VPC, Tokens, etc.)
- [x] AI PM Technical Screen (Transformers, Hallucination, LLM vs ML, Orchestration, MCP)
- [x] Industry Vertical: Hedge Funds (`/hedgefund`)
- [x] Industry Vertical: Traditional Asset Management (`/assetmanagement`)
- [x] Graph Engineering (agent graphs, LangGraph, nodes/edges/state)
- [x] GraphRAG (knowledge graphs + retrieval, multi-hop reasoning)
- [ ] Kubernetes + AWS for AI Workloads
- [ ] Industry Vertical: Legal / Law Firms
- [ ] Industry Vertical: Healthcare

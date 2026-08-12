# FDE & PM Interview Prep System

Structured preparation for **Forward Deployed Engineer (FDE)** and **Product Manager (AI)** interviews at AI-native companies — Anthropic, OpenAI, Palantir, Scale AI, Glean, Nvidia, and similar.

Built to be used with **Claude Code** (`claude` CLI). The slash commands turn Claude into a live interviewer, flashcard coach, and case study partner — all grounded in the study material in this repo.

---

## Quick Start (New User)

### 1. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

### 2. Clone this repo and open it

```bash
git clone https://github.com/majidraza1228/fde_prep.git
cd fde_prep
claude
```

### 3. Start practicing immediately

Type any slash command in the Claude Code prompt:

```
/fde
/pm
/drill
/case
/hedgefund
/concept RAG
```

That's it. Claude reads the study files in this repo and runs a live mock interview.

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
| `reference-material.md` | Official docs, links, and industry vertical index |

---

## Recommended Study Plan

### Week 1 — Build foundations
| Day | Activity |
|---|---|
| Mon | Read `fde-mock-interview-session.md` — FDE process + Areas 1–2 |
| Tue | `/drill agent` until you score 10/10 |
| Wed | Read Areas 3–4 (security, observability) → `/drill security` |
| Thu | `/fde` — full mock question, get scored |
| Fri | Read `asset-management-hedgefunds.md` → `/hedgefund` FDE mode |

### Week 2 — Add PM layer
| Day | Activity |
|---|---|
| Mon | Read PM section of `fde-mock-interview-session.md` |
| Tue | `/pm` — product sense question |
| Wed | `/drill concepts` — transformers, RAG, hallucination |
| Thu | `/concept MCP` then `/concept context engineering` |
| Fri | `/hedgefund` PM mode |

### Week 3 — Full mocks
| Day | Activity |
|---|---|
| Mon | `/fde` full session — all areas mixed |
| Tue | `/pm` full session — technical depth + product sense |
| Wed | `/case` — cold customer scenario, no help |
| Thu | `/hedgefund` — both FDE and PM mode back to back |
| Fri | `/drill` — weakest decks only, then `/case` again |

### Daily habits (5 min every morning)
Write these from memory before opening any notes:
1. The full `run_agent` function with tool result format
2. The 6 things to log on every API call
3. One-liner for: RAG vs fine-tuning, prompt caching, model tiering
4. The CISO prompt injection explanation in plain English

---

## How to Add a New Vertical

To add a new industry vertical (legal, healthcare, insurance, etc.):

1. Create `<vertical>.md` with: use cases, constraints, customer objections, FDE scenarios, PM questions, architecture pattern
2. Create `.claude/commands/<vertical>.md` following the `/hedgefund` command format
3. Add the vertical to the index in `reference-material.md`

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
- [x] Industry Vertical: Asset Management & Hedge Funds
- [ ] Kubernetes + AWS for AI Workloads
- [ ] Industry Vertical: Legal / Law Firms
- [ ] Industry Vertical: Healthcare

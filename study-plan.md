# 4-Week FDE Interview Plan — Targeting Top AI Companies

**Target companies:** Anthropic, OpenAI, Palantir, Scale AI, Databricks
**Start date:** 2026-08-23
**Goal:** Pass any FDE loop cold — code, decomp, system design, customer scenario, behavioral
**Time per day:** 60 min study + 5 min morning habit
**Method:** Learn → drill cold → score honestly → gap-fill → repeat

---

## Daily Habit (5 min every morning — write from memory, no notes)

```
1. Full run_agent() function — loop, stop_reasons, tool result format
2. One-liner: RAG vs fine-tuning vs prompt engineering — when to use each
3. Prompt caching math — $60/day → $6/day, why
4. Read tool vs write tool — gating rule
5. System prompt 5 components — persona, scope, context, tool guidance, format
```

---

## Week 1 (Aug 23–29) — Close the Code Gap

**Why first:** Every company screens on practical coding. You can't get to the interesting rounds without passing it.

| Day | Date | Focus | Command |
|---|---|---|---|
| Mon | Aug 25 | SQL — retention + window functions | `/code-drill sql` (s1, s2) |
| Tue | Aug 26 | SQL — top-N per group + cohort | `/code-drill sql` (s3, s5) |
| Wed | Aug 27 | Python — exponential backoff + pagination | `/code-drill python` (p1, p2) |
| Thu | Aug 28 | Python — CSV normalizer + JSON flattener | `/code-drill python` (p3, p4) |
| Fri | Aug 29 | Mixed SQL + Python surprise | `/code-drill sql` then `/code-drill python` |
| Sat | Aug 30 | Agent loop from memory | `/code-drill agent` |
| Sun | Aug 31 | Review gaps | Re-drill anything below Clean |

**End of week target:** Any SQL or Python drill → Clean in under 5 minutes.

---

## Week 2 (Sep 1–7) — Decomp + System Design

**Why second:** Decomposition round is the #1 rejection reason. You need 6+ reps before it's a reflex.

| Day | Date | Focus | Command |
|---|---|---|---|
| Mon | Sep 1 | Decomp intro — 5-step framework | `/decomp` (scenario A or B) |
| Tue | Sep 2 | Decomp reps | `/decomp` (scenario C or D) |
| Wed | Sep 3 | Decomp hard scenarios | `/decomp` (scenario E or F) |
| Thu | Sep 4 | Agent system design | `/fde-session fde` |
| Fri | Sep 5 | System design — RAG, VPC, security | `/fde-session fde` |
| Sat | Sep 6 | Decomp + system design back to back | `/decomp` then `/fde-session fde` |
| Sun | Sep 7 | Concept gaps | `/concept <weakest topic>` |

**End of week target:** Never say "I would build X" as your first sentence in any scenario.

---

## Week 3 (Sep 8–14) — Customer Scenarios + Behavioral

**Why third:** Heavily weighted at Anthropic (safety conviction) and OpenAI (commercial judgment).

| Day | Date | Focus | Command |
|---|---|---|---|
| Mon | Sep 8 | Client simulation — non-finance | `/case` (hospital or manufacturing) |
| Tue | Sep 9 | Client simulation — edge cases | `/case` (champion departure, board misinformation) |
| Wed | Sep 10 | Behavioral — technical judgment | `/behavioral` (q1, q2, q3) |
| Thu | Sep 11 | Behavioral — customer-facing | `/behavioral` (q5, q6, q7) |
| Fri | Sep 12 | Behavioral — FDE-specific | `/behavioral` (q13, q14) |
| Sat | Sep 13 | Finance vertical full loop | `/hedgefund` or `/assetmanagement` |
| Sun | Sep 14 | Data engineering full loop | `/dataengineering` (FDE mode) |

**End of week target:** 8 STAR stories ready. Every answer uses "I" not "we". Every result has a number.

---

## Week 4 (Sep 15–21) — Company-Specific Full Mock Loops

**One company per day. Full loop: company brief → code → decomp → scenario → behavioral.**

| Day | Date | Company | Commands |
|---|---|---|---|
| Mon | Sep 15 | Anthropic | `/company Anthropic` → `/fde-session fde` → `/behavioral` |
| Tue | Sep 16 | OpenAI | `/company OpenAI` → `/code-drill python` → `/decomp` |
| Wed | Sep 17 | Palantir | `/company Palantir` → `/decomp` (30 min timed) → `/case` |
| Thu | Sep 18 | Scale AI | `/company` Scale AI → `/fde-session fde` → `/concept evals` |
| Fri | Sep 19 | Databricks | `/company Databricks` → `/code-drill sql` → `/dataengineering` |
| Sat | Sep 20 | Full cold mock | `/fde-session fde` → `/decomp` → `/behavioral` — no help |
| Sun | Sep 21 | Gap close | Re-run any Borderline or No hire from the week |

---

## The One Rule

> Every session ends with you saying the answer out loud — not reading it, not typing it. Out loud. That's what locks it in.

---

## Commands Reference

```
/code-drill sql          SQL reps (retention, window functions, cohort)
/code-drill python       Python reps (backoff, pagination, CSV, JSON)
/code-drill agent        agent loop from memory
/decomp                  decomp round — most important
/behavioral              STAR story practice (16 questions)
/fde-session fde         random FDE mock, scored
/case                    skeptical customer role-play
/company <name>          company-specific prep
/concept <topic>         any concept at interview depth
/dataengineering         data engineering vertical
/hedgefund               hedge fund vertical
/assetmanagement         asset management vertical
```

---

# 3-Week FDE & PM Study Plan

**Goal:** Answer any FDE or PM interview question cold — without scaffolding, without hints.
**Time per day:** 60–90 min study + 5 min daily habit
**Method:** Learn → drill → mock → gap-fill → repeat

---

## Daily Habit (5 min every morning — do this before anything else)

Write these from memory. No notes. Compare after.

```
Day 1–7:   Focus habit on agent loop + cost math
Day 8–14:  Add PM one-liners + product sense structure
Day 15–21: Full set — everything from memory in 5 min
```

**The full daily habit set (build up to this by Day 15):**

| # | Write from memory |
|---|---|
| 1 | Full `run_agent` function — loop, stop_reasons, tool result format |
| 2 | The 6 things to log on every API call |
| 3 | One-liner: RAG vs fine-tuning |
| 4 | One-liner: prompt caching (what it saves, when it applies) |
| 5 | Model tiering: Haiku → Sonnet → Opus — which task gets which |
| 6 | CISO prompt injection explanation in plain English |
| 7 | North star metric definition + one example for an AI product |

---

## Week 1 — Build Technical Foundations (FDE-first)

**Theme:** Get the core FDE technical areas solid. Agent loop, cost, security, observability.

---

### Day 1 — Agent Loop (Mon)

**Read (30 min):**
- `fde-mock-interview-session.md` → FDE Interview Process section
- `fde-mock-interview-session.md` → Area 1: System Design for Agentic Systems
- `agent-loop-cheatsheet.md` → full file

**Practice (30 min):**
```
/drill agent
```
Target: 8/10 or better. If below, re-read Area 1 and retry.

**Daily habit today:**
Write the `run_agent` function from memory. Compare to `agent-loop-cheatsheet.md`.

**Key things to know by end of day:**
- [ ] `for i in range(N)` not `while True`
- [ ] Three stop_reasons: `end_turn`, `tool_use`, `max_tokens`
- [ ] Tool result format: `{"type": "tool_result", "tool_use_id": tool.id, "content": str(result)}`
- [ ] Wrap batch items in try/except

---

### Day 2 — Cost Engineering (Tue)

**Read (30 min):**
- `fde-mock-interview-session.md` → Area 2: Token Optimization and Cost Engineering (full section)
- `prompt-caching.md` → full file

**Practice (30 min):**
```
/drill cost
```

**Key things to know by end of day:**
- [ ] Prompt caching saves ~90% on cached tokens (Claude: must be >1024 tokens)
- [ ] Cache hit verification: `cache_read_input_tokens > 0`
- [ ] Cost math: 10,000 runs × 2,000 token system prompt × $3/M = $60/day → $6/day with cache
- [ ] Batch API = 50% cheaper, async only
- [ ] Model tiering: Haiku for classify/route, Sonnet for reasoning, Opus for complex only

**Daily habit today:**
Write the 5 cost reduction techniques from memory.

---

### Day 3 — Security (Wed)

**Read (30 min):**
- `fde-mock-interview-session.md` → Area 3: Security

**Practice (30 min):**
```
/drill security
```

Then say the CISO explanation out loud. Time yourself — it should take under 90 seconds.

**Key things to know by end of day:**
- [ ] Prompt injection: label untrusted data with XML tags, whitelist in code not just instructions
- [ ] Secrets: never hardcode — use `os.environ` locally, AWS Secrets Manager in production
- [ ] Tool whitelisting: `ALLOWED_TOOLS` set, reject anything outside it
- [ ] CISO explanation (memorize): "Imagine you hired an assistant and told them: only email our approved client list..."

---

### Day 4 — Observability & Debugging (Thu)

**Read (30 min):**
- `fde-mock-interview-session.md` → Area 4: Observability and Debugging

**Practice (30 min):**
```
/drill logs
/drill debug
```

**Key things to know by end of day:**
- [ ] 6 log fields: portfolio_id, input_tokens, output_tokens, stop_reason, latency_ms, tool_called
- [ ] 4-step debug: search logs → check stop_reason → check tool results → replay
- [ ] 5 failure modes: agent loops, tool failure, context overflow, prompt regression, cost spike

---

### Day 5 — Full FDE Mock (Fri)

**No reading today. All practice.**

```
/fde
```

Take it cold. Answer every question without opening your notes. Get scored.

After the mock:
- Write down every gap the mock identified
- Spend 20 min re-reading the sections where you lost points
- Update your daily habit list with anything you missed

**Target score by end of Week 1:** Borderline or better on all FDE areas.

---

### Weekend — Claude API + OpenAI Platform

**Sat (45 min):**
- Read `fde-mock-interview-session.md` → Area 5: Platform-specific Claude API
- Write `run_agent` with `cache_control` and `tool_choice` from memory

**Sun (45 min):**
- Read `fde-mock-interview-session.md` → Area 6: OpenAI Platform
- Write the OpenAI Responses API agent loop from memory

---

## Week 2 — Add PM Layer

**Theme:** Stack PM frameworks on top of the technical foundations. Every technical concept now needs a product judgment angle.

---

### Day 8 — PM Interview Process + Product Sense (Mon)

**Read (30 min):**
- `fde-mock-interview-session.md` → PM Interview Process section
- `fde-mock-interview-session.md` → PM-Specific Prep: Product Sense Answer Structure

**Practice (30 min):**
```
/pm
```
Ask for a product sense question. Answer using the 7-step structure:
Clarify → User → Pain → Solutions → Prioritize → Metrics → Risks

**Key things to know by end of day:**
- [ ] 7-step product sense structure (say it from memory)
- [ ] North star metric = value delivered, not activity
- [ ] Guardrail metric = what you watch to make sure the north star isn't gaming the system
- [ ] Anthropic PM has a standalone culture interview — know your "feature I didn't build" story

---

### Day 9 — Technical Depth at PM Level (Tue)

**Read (30 min):**
- `fde-mock-interview-session.md` → AI PM Technical Screen section (Nvidia/Glean)

**Practice (45 min):**
```
/concept transformers
/concept hallucination
/concept RAG
```

For each: read the explanation, close it, then answer cold: "How would you explain X to a PM interviewer?"

**Key things to know by end of day:**
- [ ] Transformers one-liner (memorize)
- [ ] Hallucination: model predicts probable text, not verified facts → mitigation is RAG + citations
- [ ] LLM vs traditional ML decision table (by memory)
- [ ] The rule: answer like a PM — technical explanation → user impact → product judgment

---

### Day 10 — Conflicting Metrics + Strategy (Wed)

**Read (30 min):**
- `fde-mock-interview-session.md` → North Star Metric Framework
- `fde-mock-interview-session.md` → Prioritization (RICE)
- `fde-mock-interview-session.md` → Build vs Buy vs Partner

**Practice (30 min):**
```
/pm
```
Ask for a conflicting metrics question or strategy question specifically.

**Key things to know by end of day:**
- [ ] Conflicting metrics structure: don't panic → segment → hypothesize → investigate → decide
- [ ] RICE: Reach × Impact × Confidence / Effort
- [ ] Build vs buy rule: build core differentiators, buy commodity infrastructure

---

### Day 11 — Safety, Ethics, Anthropic Culture (Thu)

**Read (20 min):**
- `fde-mock-interview-session.md` → Safety/Ethics Questions section

**Practice (40 min):**
```
/pm
```
Ask for a safety/ethics question specifically. Practice the answer frame:
Name the tension → criteria → process → own the decision → reflect

Prepare your personal story: "A feature I decided not to build and why."
Write it out. It should be 2–3 min when spoken.

---

### Day 12 — Full PM Mock (Fri)

**No reading. All practice.**

```
/pm
```

Take a full session — product sense + technical depth + safety question. Get scored on all three.

After the mock — same as Week 1 Friday:
- Write every gap
- Re-read the weak sections
- Update your daily habit list

**Target score by end of Week 2:** Pass on product sense, Borderline or better on technical depth.

---

### Weekend — Hedge Fund Vertical

**Sat (60 min):**
- Read `asset-management-hedgefunds.md` → full file
- Focus on: 4 constraints (VPC, audit trail, human-in-the-loop, regulations)
- Memorize the 5 customer objections and scripted answers

**Sun (60 min):**
```
/hedgefund
```
FDE mode first. Then PM mode. Get scored on both.

---

## Week 3 — Full Mocks + Gap Closing

**Theme:** No more reading. Every session is a cold mock. Close gaps with targeted re-reads only.

---

### Day 15 — Cold FDE Full Mock (Mon)

```
/fde
```

Full session. No notes open. Score every answer.

**After:** List your 3 biggest gaps. Spend 20 min re-reading only those sections.

---

### Day 16 — Cold Case Study (Tue)

```
/case
```

Take it completely cold. Claude plays the customer. No help.

After scoring — review: did you clarify first? handle the pushback? give a recommendation with tradeoffs?

---

### Day 17 — Cold PM Full Mock (Wed)

```
/pm
```

Full session: product sense + technical depth + safety. No notes.

After: compare your technical answers to the one-liners in the AI PM Technical Screen section.

---

### Day 18 — Hedge Fund Vertical Full Mock (Thu)

```
/hedgefund
```

FDE mode: pick the scenario you haven't done yet.
PM mode: take a product sense + safety question back to back.

Focus today: every answer must address data residency, audit trail, and human-in-the-loop.

---

### Day 19 — Weakest Area Sprint (Fri)

**Identify your weakest area from the week's mocks. Spend the full session on it.**

Options:
```
/drill agent        ← if agent loop is still shaky
/drill cost         ← if cost math gaps remain
/concept MCP        ← if PM technical depth needs work
/case               ← if customer scenario handling is weak
/hedgefund          ← if the vertical is thin
```

End the day with `/fde` or `/pm` — confirm the gap is closed.

---

### Day 20 — Simulate Real Interview Conditions (Sat)

**Set a timer. No notes. No pausing.**

Run this back to back:
1. `/fde` — answer 3 questions timed (8 min each)
2. 5 min break
3. `/pm` — answer 3 questions timed (8 min each)
4. 5 min break
5. `/case` — cold customer scenario, no help

Score everything. The goal is to feel what a real 4-hour interview loop feels like.

---

### Day 21 — Review + Final Prep (Sun)

**No mocks today. Review only.**

1. Re-read your daily habit list — can you write everything from memory?
2. Review every "Where you'd lose points" note from the week's mocks
3. Read the one-liner table in the AI PM Technical Screen section
4. Read the 5 customer objections in `asset-management-hedgefunds.md`
5. Write your "feature I didn't build" story one more time — time it

**You're ready if:**
- [ ] Agent loop: write from memory, no gaps
- [ ] Cost math: can explain 80% reduction step by step
- [ ] RAG vs fine-tuning: can convince a skeptical CTO in 3 sentences
- [ ] Hallucination: explain to a CISO, explain to a CIO, explain to a compliance officer
- [ ] Product sense: 7-step structure flows without thinking
- [ ] Safety: have a personal story ready, own the decision

---

## How to Use the Slash Commands During This Plan

| Situation | Command |
|---|---|
| Starting a new area | `/concept <topic>` — learn it first, then get quizzed |
| Drilling fundamentals | `/drill <deck>` — rapid fire, scored |
| Testing yourself | `/fde` or `/pm` — full mock question |
| Customer scenario practice | `/case` — cold, no help |
| Industry vertical practice | `/hedgefund` — FDE or PM mode |
| Reviewing a weak concept | `/concept <topic>` — explanation + cold quiz |

---

## Progress Tracker

Copy this and update after each session:

```
Week 1 — FDE Foundations
[ ] Day 1:  Agent loop — /drill agent score: ___/10
[ ] Day 2:  Cost engineering — /drill cost score: ___/10
[ ] Day 3:  Security — /drill security score: ___/10
[ ] Day 4:  Observability — /drill logs + debug score: ___/10
[ ] Day 5:  FDE full mock — score: Pass / Borderline / No hire
[ ] Sat:    Claude API written from memory: Yes / No
[ ] Sun:    OpenAI loop written from memory: Yes / No

Week 2 — PM Layer
[ ] Day 8:  Product sense — /pm score: ___
[ ] Day 9:  Technical depth — /concept x3 score: ___
[ ] Day 10: Strategy — /pm conflicting metrics score: ___
[ ] Day 11: Safety story written and timed: Yes / No
[ ] Day 12: PM full mock — score: Pass / Borderline / No hire
[ ] Sat:    Hedge fund objections memorized: Yes / No
[ ] Sun:    /hedgefund both modes score: ___

Week 3 — Full Mocks
[ ] Day 15: Cold FDE mock — score: ___
[ ] Day 16: Cold case study — score: ___
[ ] Day 17: Cold PM mock — score: ___
[ ] Day 18: Hedge fund full mock — score: ___
[ ] Day 19: Weakest area sprint — gap closed: Yes / No
[ ] Day 20: Simulated interview loop — overall: ___
[ ] Day 21: All daily habits from memory: Yes / No
```

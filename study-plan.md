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

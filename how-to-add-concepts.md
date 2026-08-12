# How to Add a New Concept to the Prep System

When you encounter a new concept in an article, LinkedIn post, job description, or interview — add it to the system in 3 steps.

---

## Step 1 — Add the Concept Explanation

Open `fde-mock-interview-session.md` and find:
```
## Foundational Concepts — FDE & PM Must Know
```

Add a new `###` section using this exact template:

```markdown
### [Concept Name]

**One-liner (memorize this):**
> "Single sentence that passes the depth filter."

**One level deeper:**
2–3 sentences of how it actually works. Mechanism, not definition.

**Why it matters — FDE angle:**
How this shows up in customer deployments, on-site scenarios, or production systems.

**Why it matters — PM angle:**
How this shapes product decisions, metrics, tradeoffs, or user trust.

**Interview answer (say this out loud):**
3–5 sentences combining technical depth + product judgment. This is what you memorize.

**Follow-up the interviewer will ask:**
> "The pushback question they always ask next."

> "The answer to that pushback."

**Reference resources (optional):**
- [Title — Source](URL)
```

**Rules for the template:**
- One-liner must be memorizable in one read
- "One level deeper" is technical — mechanism not definition
- FDE and PM angles must be different — don't repeat yourself
- Interview answer must be speakable in under 60 seconds
- Always include the follow-up — that's the depth filter moment

---

## Step 2 — Add to the `/concept` Command

Open `.claude/commands/concept.md` and find the concepts list:

```
## Concepts this command covers
```

Add your new concept to the comma-separated list. This tells Claude it can be quizzed.

---

## Step 3 — Add to the Study Plan (if it's a priority concept)

Open `study-plan.md` and decide where it fits:

- **Week 1 (FDE foundations):** Agent-related, cost, security, observability
- **Week 2 (PM layer):** Product metrics, technical depth concepts, strategy
- **Week 3 (mocks):** Just drill it using `/concept <name>`

Add it to the relevant day's "Key things to know" checklist.

---

## Quick Version (when you're in a rush)

Just type in Claude Code:
```
/concept [new concept name]
```

Claude will explain it using the standard format AND offer to save it to `fde-mock-interview-session.md` automatically. Say yes and it's done.

---

## How to Add a New Industry Vertical

When you want to add a vertical (legal, healthcare, insurance, etc.):

### 1. Create the vertical file

Copy the structure of `asset-management-hedgefunds.md`:

```markdown
# [Vertical] — FDE & PM Prep

## Industry Overview
- How the industry works (roles, workflows, pain points)
- Types of companies in this vertical

## Key AI Use Cases
- 4–6 use cases with FDE deployment pattern + PM product angle

## Key Constraints
- Data residency, compliance, audit, human-in-the-loop

## Common Customer Objections
- 4–5 objections with scripted FDE answers

## FDE Mock Scenarios
- 3–5 on-site scenarios

## PM Mock Questions
- Product sense, strategy, safety questions set in this vertical

## Technical Architecture
- Diagram specific to this industry

## Key Numbers to Drop in Interviews
- Stats that make you sound like you know the space

## Reference Resources
```

### 2. Create the slash command

Copy `.claude/commands/hedgefund.md` and update:
- The company/customer names and roles
- The 5 FDE scenarios with industry-specific constraints
- The PM questions with industry-specific framing
- The scoring checklist (what must every answer address in this vertical?)

Name the file `.claude/commands/[vertical].md`

### 3. Update `reference-material.md`

Add the vertical to the Industry Verticals table.

### 4. Update `README.md`

Add the new command to the slash commands table and the Areas Covered checklist.

---

## Concept Addition Log

Track new concepts added so you can see how the system grows:

| Date | Concept | Source | File updated |
|---|---|---|---|
| 2026-08-11 | Graph Engineering | Analytics Vidhya / AI Builder Club | `fde-mock-interview-session.md` |
| 2026-08-11 | GraphRAG | MyEngineeringPath / Medium | `fde-mock-interview-session.md` |
| — | Add yours here | — | — |

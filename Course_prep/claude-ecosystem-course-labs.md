# The Claude Ecosystem for PMs — Lab Guide
### Companion to `claude-ecosystem-course-for-pms.md`

7 labs, ~110 minutes total, across 4 classes. Every lab runs on the participant's real work — their own PRDs, docs, product space, and team context. No toy examples.

**What participants need before Class 1:** a Claude.ai account (Pro or Max tier — Cowork, Claude Code, and MCP connectors require a paid tier), one real work document to upload (a PRD, brief, or strategy doc), and admin/owner access to at least one workplace tool they'd want to connect later (Slack, Google Drive, Notion, etc.) for Lab 2B.

---

## Lab 1A — PM Workspace Setup
**Class 1 · 20 min**

**Objective:** leave with a real, configured Claude workspace — not a demo account.

**Prerequisites:** Claude.ai account, one real work document ready to upload.

**Steps**
1. Create a Project in Claude.ai named for your actual role or team (e.g. "Growth PM — Acme Co," not "Test Project").
2. Write real Project instructions: your product area, your team's terminology, the format you want outputs in, and 2–3 things Claude should never assume (e.g. "always flag when you're speculating about user data we don't have").
3. Upload your real document(s) — PRD, roadmap, competitive notes, whatever you'd actually reference day to day.
4. Open Settings → Memory and audit what Claude has already inferred about your work. Correct anything wrong; add anything missing (your role, your product, your team structure).
5. Ask Claude to produce one real Artifact from your uploaded material — e.g. "turn this into a one-page competitive summary" — and review it critically against what you know is true.

**Deliverable:** a configured Project (real instructions + real docs), an audited Memory profile, and one Artifact built from real material.

**Facilitator notes:** the failure mode here is participants using placeholder content "to be safe." Push back on that in the room — vague instructions produce vague labs 2 through 7.

---

## Lab 2A — Build Your PM Skill
**Class 2 · 15 min**

**Objective:** scaffold and run a Skill built from your own methodology, not a generic template.

**Prerequisites:** Lab 1A completed; a repeatable task you do often (PRD formatting, discovery interview guide, competitive brief, status report).

**Steps**
1. Pick one recurring task you do at least monthly and could describe as a repeatable checklist.
2. Write out your actual method in plain language: the sections you always include, the order you do things in, the phrasing or tone you default to, and any hard rules (e.g. "always include a risks section").
3. Use a skill-creation workflow to turn that into a Skill — a name, a trigger description (when should Claude reach for this automatically), and the instructions themselves.
4. Test it on a real input — not a hypothetical. If you chose "PRD formatter," run it on an actual half-finished PRD.
5. Compare the output against what you'd have written manually. Note where it diverged from your intent and tighten the Skill's instructions.

**Deliverable:** one working Skill, tested against real input, with at least one revision based on the test.

**Facilitator notes:** the ~100-token trigger description is the part people under-invest in — spend 5 minutes just on getting the trigger description right so the Skill fires when it should and stays quiet otherwise.

---

## Lab 2B — Agentic Chain
**Class 2 · 15 min**

**Objective:** connect real MCP servers and run one task that crosses systems without manual copy-paste.

**Prerequisites:** admin/owner access to at least one of: Slack, Google Drive, Notion (or another connector available to you); Lab 2A completed.

**Steps**
1. Connect 3 MCP servers relevant to your actual workflow — e.g. Slack (for team context), Drive or Notion (for docs), and web search (for external context).
2. Design one task that genuinely needs all three — not one that could be done with a single tool. Example: "search our Slack for the last discussion of [feature], pull the related doc from Drive, cross-reference with what's publicly known about [competitor], and draft a one-paragraph recommendation."
3. Run it and watch where Claude needs course-correction — wrong channel searched, wrong doc version pulled, ambiguous instructions.
4. Rewrite the instruction to close that gap and re-run.

**Deliverable:** a completed cross-system task, plus a one-line note on where course-correction was needed and how the instruction was fixed.

**Facilitator notes:** have a fallback ready — connector auth is the most common live-session failure point. Prepare a sandbox account with pre-connected servers so a participant with broken auth can still complete the exercise conceptually.

---

## Lab 3A — First Claude Code Task
**Class 3 · 15 min**

**Objective:** run a real Claude Code task, choosing difficulty based on comfort level.

**Prerequisites:** Claude Code installed (guide provided before class); Labs 1A/2A completed.

**Choose one tier:**
- **Tier 1 — Research synthesis:** point Claude Code at a folder of your own notes/docs and ask it to synthesize findings into a brief.
- **Tier 2 — Document processing:** have it batch-process or restructure a set of real files (e.g. reformat meeting notes into a consistent template).
- **Tier 3 — Spec-to-prototype:** hand it a real spec you've written and ask it to build a rough prototype or mockup from it.

**Steps**
1. Pick your tier honestly — this isn't scored, and Tier 1 done well beats Tier 3 done badly.
2. Write a CLAUDE.md for the working folder: what this project is, what "done" looks like, any constraints.
3. Run the task and watch the agentic loop — where does it ask for clarification, where does it just proceed?
4. Review the output against your own judgment of quality, not just "did it run."

**Deliverable:** a completed task at your chosen tier, plus the CLAUDE.md you wrote for it.

**Facilitator notes:** let people self-select tiers without pressure — the point is a real completed task, not the hardest one.

---

## Lab 3B — Autonomous Research Loop
**Class 3 · 20 min**

**Objective:** run a full autonomous research loop on your own product space, and leave with a CLAUDE.md that lets a future session resume it.

**Prerequisites:** Lab 3A completed.

**Steps**
1. Define a real research question about your product space — e.g. "how are our top 3 competitors positioning their new AI features this quarter."
2. Set up a working folder with a CLAUDE.md describing: the research goal, sources to prioritize, output format, and what "good enough" looks like.
3. Run the loop and let it work with minimal intervention — resist the urge to micromanage each step.
4. When it finishes (or you stop it), review the output and update the CLAUDE.md with what you'd tell a future session to do differently — this is the actual deliverable, not just the research output.

**Deliverable:** a completed research artifact + a CLAUDE.md written so that tomorrow's session could pick up where today's left off.

**Facilitator notes:** this lab often overruns 20 minutes — plan for participants to finish the loop async and bring results to the Class 4 discussion.

---

## Lab 4A — Full Cowork Task
**Class 4 · 15 min**

**Objective:** complete one real PM deliverable end-to-end using Cowork's full capability, and compare it to your manual process.

**Prerequisites:** Cowork access; Labs 1A–3B completed (this lab draws on the workspace, Skills, and connectors already set up).

**Choose one deliverable:**
- A synthesis brief pulling from multiple real sources
- A stakeholder deck outline for an actual upcoming meeting
- An executive summary of a real initiative

**Steps**
1. Pick a deliverable you actually owe someone this week or next — real stakes produce real evaluation.
2. Give Cowork the full task, including connectors and files it needs, and let it work through the sandboxed environment rather than doing each step yourself in chat.
3. Review the output as if you were about to send it — what needs a real edit vs. what's ready.
4. Note, in one paragraph, how this compares in time and quality to how you'd normally produce this deliverable.

**Deliverable:** one real, near-ship-ready PM deliverable, plus a short manual-process comparison.

**Facilitator notes:** the comparison note is what participants reference later when deciding whether to adopt this into their real workflow — don't let people skip it.

---

## Lab 4B — Capstone: Design Your Team's Claude Stack
**Class 4 · 10 min in-class + take-home**

**Objective:** produce an actionable blueprint for your team's or company's Claude deployment.

**Prerequisites:** all prior labs completed — this lab synthesizes them.

**Steps**
1. Using the Team Stack Design worksheet, map out: which surfaces your team needs (Projects, Cowork, Claude Code), which Skills would standardize your team's recurring work, which MCP connectors matter most, and what guardrails you'd set (data access, review requirements, spend limits).
2. If run as a group workshop, pair up and pressure-test each other's blueprint — where would this actually break in your org?
3. Identify the first rollout step: who pilots it, what gets measured, what "success" looks like in 30 days.

**Deliverable:** a completed Team Stack Design worksheet with a concrete first rollout step — this is the artifact participants take back to leadership.

**Facilitator notes:** this is the "leave with a real deployment, not a slide deck" promise — hold the bar high here. A blueprint that just lists tool names without guardrails or a rollout owner isn't done.

---

## Lab Summary Table

| Lab | Class | Time | Deliverable |
|-----|-------|------|-------------|
| 1A — PM Workspace Setup | 1 | 20 min | Configured Project + audited Memory + one real Artifact |
| 2A — Build Your PM Skill | 2 | 15 min | One tested Skill built from your own methodology |
| 2B — Agentic Chain | 2 | 15 min | Cross-system task via 3 connected MCP servers |
| 3A — First Claude Code Task | 3 | 15 min | Completed task at chosen difficulty tier + CLAUDE.md |
| 3B — Autonomous Research Loop | 3 | 20 min | Research artifact + resumable CLAUDE.md |
| 4A — Full Cowork Task | 4 | 15 min | Real PM deliverable + manual-process comparison |
| 4B — Capstone | 4 | 10 min + take-home | Team Stack Design blueprint with rollout step |

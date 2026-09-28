# Technical CSM Interview Guide — Kong AI Gateway
### ApexTrans Logistics scenario · Interviewer copy (with model answers)

---

## Overview

This guide is designed to evaluate a Technical CSM candidate using the ApexTrans Logistics × Kong AI Gateway scenario. The candidate has been given the deck and asked to prepare a demo. Questions test technical depth on Kong's AI plugins, customer success judgment, demo composure, and objection handling.

**Suggested 60-minute flow:**
- 5 min — introductions
- 15 min — Technical questions (pick 3)
- 10 min — Customer Success questions (pick 2)
- 10 min — Demo / presentation questions
- 10 min — Objection handling (pick 2)
- 5 min — one Behavioural question
- 5 min — candidate questions

---

## Scoring Rubric

| Area | Weight | What separates good from great |
|------|--------|-------------------------------|
| Technical depth | 30% | Can explain plugin mechanics, not just feature names |
| CSM judgment | 30% | Diagnoses before prescribing; thinks in outcomes not features |
| Communication | 20% | Shifts register by audience; stays calm under pressure |
| Demo readiness | 10% | Has a backup plan; doesn't over-demo |
| Honesty | 10% | Says "I'd confirm that" rather than guessing |

---

## Quick Reference: Strong vs Weak Signals

| Signal | Strong candidate | Weak candidate |
|--------|-----------------|----------------|
| Technical depth | Explains mechanisms, not feature names | Recites slide bullets |
| Objection handling | Reframes, uses data, acknowledges tradeoffs | Deflects or over-promises |
| Discovery | Asks about pain and urgency | Asks about budget and timeline |
| Demo failure | Narrates it as a product strength | Apologises and fumbles |
| Gaps in knowledge | "I'd confirm that and follow up" | Guesses confidently |
| CSM motion | Diagnoses before prescribing | Immediately prescribes features |
| Communication | Shifts vocabulary by audience | Same pitch for all stakeholders |

---

## Section 1: Technical Depth

---

### T1. AI Proxy vs AI Proxy Advanced

**Question:**
> "What's the difference between AI Proxy and AI Proxy Advanced, and when would you recommend each?"

**Model answer:**
AI Proxy is a single-upstream plugin. You point it at one model provider — OpenAI, Anthropic, Azure, etc. — Kong holds the API key and translates the request. It handles provider abstraction and key security, nothing more.

AI Proxy Advanced adds: weighted load balancing across multiple providers, priority-ordered fallback (try OpenAI first, fall back to Azure if it returns a 429 or 5xx), retries with backoff, and the ability to route different request types to different models based on cost or capability.

Recommend AI Proxy for a single-model proof of concept or a team that only ever calls one provider. Recommend AI Proxy Advanced to any production account with resilience requirements, multiple providers, or cost routing needs — which is essentially every enterprise customer.

For ApexTrans specifically, with 20 squads and a shared OpenAI key in the demo, Advanced is the right answer from day one.

**Follow-up probe:** "What's the fallback behaviour when the primary model returns a 429 — does the client see the error?"

**Strong answer:** No. Kong retries transparently to the client. The client gets a response from the fallback provider. The 429 is logged and observable in Konnect analytics but the client experience is uninterrupted.

**Weak answer:** Candidate says the client gets the 429 and retries themselves, or doesn't know the retry is transparent.

---

### T2. AI Rate Limiting Advanced — token semantics

**Question:**
> "Does the rate limiter count input tokens, output tokens, or both? What happens to a request in flight when the budget is exhausted?"

**Model answer:**
Both. AI Rate Limiting Advanced counts prompt tokens (input) and completion tokens (output) separately or together depending on configuration. The plugin intercepts the response from the upstream model to count output tokens, since those aren't known until the response arrives.

When the budget is exhausted, the next incoming request is blocked with a 429 and a Retry-After header. A request already in flight completes — Kong does not terminate a streaming response mid-stream.

Configuration levers: window (per minute, hour, or day), budget size per consumer group or per model, and whether to count input only, output only, or combined. For ApexTrans dispatch copilot during an active shift, the right design is a per-hour window with a generous burst allowance so a single busy period doesn't hard-stop operations.

**Follow-up probe:** "If the dispatch squad has a 50,000 token/hour budget and a single query uses 48,000 tokens, what happens to the next query?"

**Strong answer:** It gets blocked immediately, since only 2,000 tokens remain. The candidate should then suggest either a higher budget, a per-request cap to prevent any single query from consuming the budget, or graceful degradation to a cheaper model via AI Proxy Advanced when the budget is low.

**Weak answer:** Candidate isn't sure or says Kong would split the request.

---

### T3. AI Semantic Cache

**Question:**
> "How does semantic caching work, and what are the failure modes?"

**Model answer:**
When a prompt arrives, Kong generates a vector embedding of it and queries a cache store (Redis by default). If a stored embedding is within a configurable similarity threshold — say 0.95 cosine similarity — Kong returns the cached response without calling the upstream model. If not, it calls the model, returns the response, and stores the embedding for future hits.

The practical benefit for ApexTrans: support bots fielding "Where is load 4471?" repeatedly will hit the cache after the first call. Shipment status questions have high semantic similarity even when phrased differently.

**Failure modes:**
1. **Stale data.** Shipment status changes in real time. A cached answer from 10 minutes ago could be wrong. The fix is a low TTL or disabling semantic cache for routes where freshness matters.
2. **Threshold tuning.** Set the threshold too low and you get false positive cache hits — semantically different questions returning wrong answers. Set it too high and the cache hit rate drops to near zero.
3. **Cold start.** The cache is empty at launch. For the first few days, every request is a miss and you're paying the embedding generation cost with no benefit.
4. **Embedding model dependency.** If the embedding model is unavailable, the cache lookup fails and Kong falls through to the upstream model. This should be the designed behaviour, not an outage.

**Follow-up probe:** "Would you enable semantic cache on the exception-handling agent route?"

**Strong answer:** Probably not, or with a very low TTL. Exception handling requires real-time freight data. A cached rerouting recommendation for a load that's already been resolved is actively harmful.

**Weak answer:** Candidate says yes without qualifying the freshness risk.

---

### T4. AI Prompt Guard vs AI Semantic Prompt Guard

**Question:**
> "Why use both plugins on the same route? Can't Semantic Prompt Guard replace the regex one?"

**Model answer:**
They serve different attack surfaces. AI Prompt Guard uses regex patterns — it blocks exact strings or known attack signatures like "ignore previous instructions" or "system:" injections. It runs entirely within Kong, adds sub-millisecond latency, and costs nothing per call.

AI Semantic Prompt Guard sends the prompt to a classifier model to evaluate intent. It catches novel phrasing, obfuscated injection, and attacks deliberately rephrased to avoid regex detection. The tradeoff is it adds latency (an extra model call) and costs tokens.

Running both is defence in depth: regex catches the cheap obvious attacks first. Semantic catches the sophisticated ones regex misses. You don't pay for a semantic evaluation on a prompt that was already blocked by regex.

Semantic Prompt Guard cannot fully replace regex Guard because: it adds cost to every uncached request, it has a small false positive rate, and for compliance purposes many customers need deterministic pattern-based blocking they can audit.

**Follow-up probe:** "A new injection technique is discovered that bypasses both. What's the next line of defence in the ApexTrans architecture?"

**Strong answer:** Response-side content safety integrations — the deck references Azure AI Content Safety and AWS Bedrock Guardrails on the response path. Even if an injected prompt gets through, the response can be intercepted before it reaches the client. Full request/response logging also means the attack is visible for incident response.

**Weak answer:** Candidate says to update the regex patterns and stops there.

---

### T5. AI MCP Proxy — concrete mechanics

**Question:**
> "Walk me through exactly what happens when an agent makes a tool call to the shipment API through Kong."

**Model answer:**
Without MCP Proxy, an agent that wants to call the shipment API would need a separate MCP server sitting in front of that API — translating between the MCP tool-call format and the REST API. That's new infrastructure every team has to build and maintain.

With AI MCP Proxy, Kong exposes the existing shipment REST API as an MCP tool. Here's the sequence:

1. The agent sends an MCP tool call: `{"tool": "get_shipment_status", "input": {"load_id": "4471"}}`.
2. Kong's MCP Proxy receives it, matches it to the configured tool definition, and translates it to `GET /shipments/4471` on the internal shipment API.
3. Kong applies auth (the agent's credential), ACL (does this agent have permission to call read tools on the shipment service?), and rate limits.
4. The shipment API responds with JSON. Kong wraps it back into an MCP tool result and returns it to the agent.
5. Everything is logged — the tool call, the credential used, the response — for audit.

The shipment API team changes nothing. The agent team gets a governed, audited path to real data without building an MCP server.

**Follow-up probe:** "What happens if an agent tries to call a write tool — like update shipment status — but its credential only has read scope?"

**Strong answer:** Kong's ACL blocks it and returns a 403. The agent never reaches the API. The block is logged. This is the "write tools need stricter scopes and approval" point — the agent identity credential maps to per-tool ACLs configured in Kong.

**Weak answer:** Candidate says the API itself handles authorisation and Kong just passes it through.

---

### T6. OpenAI-compatible endpoint — SDK compatibility

**Question:**
> "A squad uses the Anthropic Python SDK directly. How do they adopt Kong's AI Gateway without rewriting their code?"

**Model answer:**
This is a real friction point and deserves an honest answer.

The Anthropic Python SDK uses Anthropic's native wire format — it is not OpenAI-compatible. Simply changing the base URL will not work because the request and response schema are different.

There are three paths:

1. **Switch to an OpenAI-compatible client.** The squad replaces `import anthropic` with `import openai` and points it at the Kong base URL. Kong's AI Proxy translates to Anthropic's API on the backend. The squad loses Anthropic-specific features not in the OpenAI schema (extended thinking, etc.) but gains the governance layer.

2. **Use the Anthropic SDK with Kong's Anthropic passthrough.** AI Proxy can be configured with Anthropic as the upstream provider. Kong holds the Anthropic key. The squad can still use the Anthropic SDK if Kong is configured to accept the native Anthropic format on that route.

3. **Don't change the SDK — add Kong's plugins to the Anthropic route as-is.** For squads where switching SDKs is high friction, configure Kong to inspect and apply guardrails to the Anthropic-format traffic without forcing an OpenAI-compatible wrapper.

Path 1 is the marketing simplification. Path 2 or 3 is what you'd actually implement for a squad on the Anthropic SDK.

**Follow-up probe:** "Do you surface this SDK friction upfront in the sales cycle?"

**Strong answer:** Yes. Surfacing it early builds trust and avoids a surprise during implementation. Frame it as: "For teams already on OpenAI or using OpenAI-compatible clients, the migration is one line. For teams on native Anthropic SDK, there's a small config change on the Kong route — we'd walk your platform team through it."

**Weak answer:** Candidate hides the friction or doesn't know the distinction.

---

### T7. decK and APIOps

**Question:**
> "What is decK, and does it replace Terraform?"

**Model answer:**
decK is Kong's declarative configuration tool. You define your Kong objects — services, routes, plugins, consumers, credentials — in YAML or JSON. Running `deck sync` pushes that state to the Konnect control plane. Running `deck diff` shows what would change before you apply it. It integrates into CI/CD so a PR to the Kong config repo automatically validates and deploys changes.

Terraform manages infrastructure: it provisions Konnect organisations, data plane nodes, cloud networking, Kubernetes namespaces. decK manages Kong configuration on top of that infrastructure.

They are not interchangeable. Terraform doesn't know about Kong routes and plugins at the object level. decK doesn't provision cloud resources. Most enterprise customers use both: Terraform to stand up the environment, decK to manage the API configurations day-to-day.

For ApexTrans, the message is: your existing Terraform pipelines keep working. decK plugs into the same CI/CD process your platform team already uses, so guardrail changes go through code review and have an audit trail — not a manual click in the UI.

**Follow-up probe:** "A developer accidentally syncs a decK file that removes a production route. How does Kong handle that?"

**Strong answer:** decK is destructive by default — it will remove objects not in the file. The mitigation is `deck sync --select-tag` to scope the sync to tagged objects only, plus a `deck diff` step in CI that requires approval before sync. Kong itself has no rollback button, so the real safeguard is the Git history and a fast re-sync from the previous commit.

**Weak answer:** Candidate says Kong has a rollback feature or doesn't know decK can delete objects.

---

## Section 2: Customer Success

---

### C1. Opening discovery — three questions

**Question:**
> "What three questions do you ask before showing a single slide?"

**Model answer:**

1. **"What triggered this conversation right now?"** — Understand the forcing function. Is it a security incident, a cost overrun, a board mandate, a competitor doing something, an internal audit? The answer tells you what pain is hot and what the decision timeline is.

2. **"Which teams are already using AI in production today, and which are in flight?"** — This maps the urgency and the scope. If two squads are already live with ad hoc OpenAI calls, the security risk is real today, not theoretical.

3. **"What does success look like for you personally in the first 90 days?"** — Separate the CTO's answer from the platform lead's and the developer's. Misalignment here is where implementations stall.

**What separates a strong answer:** The candidate asks questions that reveal pain, urgency, and stakeholder definition of success — not questions that are really just features dressed as questions.

**Weak answer:** Candidate asks about budget, timeline, and existing vendors — standard sales questions with no diagnostic value.

---

### C2. Stakeholder mapping

**Question:**
> "CTO, platform engineering lead, developer advocate in the room. How does your message shift for each?"

**Model answer:**

**CTO:** Cares about strategic risk, compliance exposure, and ROI. Lead with the governance and audit story — 20 squads each managing their own provider keys is a security and compliance liability. Kong gives the board a single audit trail for all AI usage.

**Platform engineering lead:** Cares about operational burden. The message is: you're already running Kong for 200 APIs. AI Gateway is an extension of what you have, not a new system to operate. You get the same decK/Terraform workflow, the same observability stack, the same on-call runbook.

**Developer advocate:** Cares about developer experience and not being the bottleneck. The message is: squads get credentials and docs from the Dev Portal and onboard themselves. No ticket to the platform team, no waiting for a key. The squad writes one curl command and they're live.

**What separates a strong answer:** The candidate explicitly references specific slides or features for each audience rather than speaking generically.

**Weak answer:** Candidate gives the same product pitch to all three with slightly different vocabulary.

---

### C3. Rollout stall — 4 of 20 squads at Month 4

**Question:**
> "Only 4 of 20 squads onboarded. Leadership is impatient. What do you do?"

**Model answer:**

First, diagnose before prescribing. The stall could be: awareness (squads don't know onboarding is open), friction (the process is too complex), competing priorities (squads are in sprint cycles), or missing value signal (they don't see why they should bother yet).

The diagnostic: pull Konnect analytics for the 4 live squads and make the numbers visible. Then interview two squads that haven't onboarded and ask what's in the way.

Concrete actions:
- **Publish pilot results** as an internal case study shared by the champion to all squad leads.
- **Identify 3-4 squads with active AI projects** in the current sprint — reach out directly rather than waiting.
- **Run a Kong Academy enablement session** — 60 minutes, hands-on, reduces activation effort.
- **Build a single-page onboarding checklist** — if the process requires more than 30 minutes of a developer's time, simplify it.
- **Set a 30-day milestone with the sponsor** — agree on the next 4 squads to onboard by name, with dates.

Avoid escalating to the CTO before diagnosing the cause. That uses political capital and may not fix the actual problem.

**Follow-up probe:** "What if the platform engineering lead is the blocker — they're too busy to support onboarding requests?"

**Strong answer:** Work with the platform lead to create self-service assets (runbooks, Dev Portal documentation) so squads don't need platform team involvement for standard onboarding. Escalate to the CTO only if the platform lead's bandwidth is genuinely blocking the account outcome, not as a first move.

---

### C4. QBR at Month 6

**Question:**
> "What data do you pull from Konnect, and what story do you tell?"

**Model answer:**

**Data to pull:**
- Squads onboarded: target vs. actual
- Total AI requests through the gateway and trend over time
- Token spend per squad vs. agreed budget — who is over, who is under
- Cache hit rate — tokens and cost avoided through semantic cache
- Blocked events: injection attempts, PII masking events, 429s from rate limiting
- AI route availability — uptime of AI Gateway routes
- Time to onboard new squads — trending down as the process matures

**Story structure:**
1. Here's what we agreed at the start — reference the KPI slide from the deck
2. Here's where we are — 2-3 numbers that show momentum
3. Here's what's still open and why — honest about the stall, with a clear plan
4. Here's the ask for the next phase — specific: "We need two named squad leads to commit to the Govern phase by end of month"

**What separates a strong answer:** The candidate names specific Konnect metrics, not generic "usage data." They structure the QBR around the outcomes the customer committed to at the start, not around Kong features.

---

### C5. Champion gone, contact dark

**Question:**
> "Platform lead stops responding. Champion moved to a different team. What do you do?"

**Model answer:**

Treat it as a churn signal on day one of the silence, not after two missed follow-ups.

Immediate actions:
1. **Reach the original champion before they fully disengage** — ask for an introduction to their replacement and a 15-minute handoff call.
2. **Map new stakeholders** — use LinkedIn, the org chart, your AE's contacts. Identify who now owns the AI platform initiative.
3. **Review Konnect usage data** — is usage still growing, flat, or dropping? A usage drop is a harder signal than silence alone.
4. **Loop in your AE** — this is a joint account problem. The AE may have exec relationships you don't.
5. **Do not send three more emails to the dark contact** — it signals desperation and doesn't solve the problem.

If you cannot find a new internal champion within 30 days, escalate internally. A renewal without a champion is at high risk.

**Follow-up probe:** "Usage is still up but you still can't get a meeting. Is the account at risk?"

**Strong answer:** Usage being up is a positive signal but not sufficient. If nobody is willing to meet, you have no visibility into upcoming budget reviews, reorgs, or competitive evaluations. The account can still churn at renewal if an executive decides AI strategy is moving in a different direction.

---

## Section 3: Demo and Presentation

---

### D1. Live failure — API key hits rate limit

**Question:**
> "The shared OpenAI key returns a 429 mid-demo. What do you do?"

**Model answer:**

Stay calm, narrate it as a feature, recover cleanly.

**In the moment:** "This is actually a scenario we planned for — you can see the 429 here, which is exactly what triggers the AI Proxy Advanced fallback. Let me show you what happens next." Then either switch to the fallback model live (if configured), or switch to a pre-recorded backup clip of the same flow.

**Preparation that prevents this:** Test the key 30 minutes before the demo. Record a full backup video of every demo segment while it works. Use saved Insomnia or curl scripts so commands are copy-paste, not typed live. Configure AI Proxy Advanced with a fallback to a second provider before the demo.

**What the panel is actually evaluating:** Not whether the demo works, but how the candidate handles pressure. A CSM who panics in a demo will panic in a customer escalation.

**Weak answer:** Candidate apologises, fumbles, tries to fix the key live, loses 5 minutes of their 12-minute window.

---

### D2. Reference architecture to a CFO in 60 seconds

**Question:**
> "Explain the reference architecture slide to a CFO who doesn't know what a data plane is. You have 60 seconds."

**Model answer:**

"Right now, each of your 20 teams has a direct connection to OpenAI, Anthropic, or whatever model they picked. That means 20 places where API keys can leak, 20 separate cost centres nobody can see across, and 20 different approaches to blocking a bad prompt.

Kong sits in the middle — one front door. Every team goes through it. Your platform team sets the security rules, the spending limits, and the compliance requirements once. Every team gets them automatically, whether they're building a support bot or a freight routing agent.

From a cost perspective: you see every token spent, by which team, on which model, in one dashboard. That's the basis for your budget conversations with each squad lead."

**What separates a strong answer:** No technical jargon. Uses the customer's language — freight, squads, compliance, budget. Connects directly to the CFO's concerns, not to product features.

**Weak answer:** Candidate uses "control plane," "plugin," or "upstream" without translating them, or describes the architecture from left to right as a technical diagram.

---

### D3. Unrequested feature — Semantic Cache

**Question:**
> "Nobody asked about AI Semantic Cache. Do you bring it up?"

**Model answer:**

Only if it connects to a specific pain the customer mentioned in discovery.

If the customer said "our support bot handles hundreds of the same shipment status questions every day and the cost is adding up" — then yes: "Earlier you mentioned the volume of repeat questions in the support bot. This is the feature that addresses that."

If no such pain was mentioned, skip it. Demoing features for their own sake signals that you're a feature demonstrator, not a problem solver.

The rule: every feature shown should connect to a pain the customer surfaced. If you can't complete the sentence "I'm showing you this because you told me X," don't show it.

**Follow-up probe:** "You have 4 minutes left and haven't covered semantic cache. You think it's genuinely relevant. What do you do?"

**Strong answer:** Cut a less relevant segment rather than rushing. A demo that goes over time and feels chaotic is worse than a demo that ends slightly early and leaves room for questions.

---

## Section 4: Objection Handling

---

### O1. Build vs buy

**Question:**
> "Our platform team says they can build this in Python in two weeks. Why pay for Kong?"

**Model answer:**

"Two weeks to build it, and then what? Someone owns it forever. Every time OpenAI changes their API, someone updates it. Every time a squad onboards, someone adds a new route. Every time there's a security incident, someone is on call.

You're not buying features — you're buying the operational burden you're not taking on. Your platform team's time has a cost. If they're maintaining a homegrown proxy, they're not building the AI features that differentiate ApexTrans.

And there's a second consideration: you're already running Kong for 200 APIs. AI Gateway runs on the same runtime. Your team already knows how to operate it, monitor it, and run it in production. A homegrown Python proxy is net-new infrastructure with net-new operational risk."

**What separates a strong answer:** Candidate shifts from feature comparison to total cost of ownership and opportunity cost. They use the existing Kong footprint as a differentiator — this is not a new vendor relationship.

**Weak answer:** Candidate lists features the Python proxy wouldn't have. The developer will respond "I can build those too."

---

### O2. Vendor lock-in

**Question:**
> "Aren't we just trading OpenAI lock-in for Kong lock-in?"

**Model answer:**

"The abstraction actually works in your favour here. Right now, if Anthropic releases a model that's twice as good at half the cost, you'd have to update 20 codebases to adopt it. With Kong in the middle, you update one config line in Konnect and every squad gets the new model.

Kong's AI Proxy uses the OpenAI-compatible wire format — which is becoming the industry standard. Your application code doesn't change. The Kong configuration is declarative YAML in your Git repo, not a proprietary binary format.

The real lock-in question is: does Kong prevent you from calling AI providers directly? No. If you decided to remove Kong tomorrow, your squads go back to direct calls. The Kong config is version-controlled and auditable. That's the opposite of lock-in."

**What separates a strong answer:** Candidate reframes lock-in as the customer's current state (20 codebases tied to provider-specific SDKs), not the future state with Kong. They also honestly acknowledge Kong is a dependency and explain why the tradeoff is worthwhile.

---

### O3. Latency

**Question:**
> "Every request goes through an extra hop. What's the latency overhead, and is it acceptable for real-time freight routing?"

**Model answer:**

"For an LLM call, the model inference time dominates completely — 500 milliseconds to several seconds for a response. Kong's data plane adds single-digit milliseconds for routing and plugin execution. That's a rounding error.

For your exception-handling agent where latency matters most, the data plane can be co-located with your application workloads — same cluster, same region. The network hop is then microseconds.

And semantic caching actually reduces end-to-end latency for repeat queries. Instead of an 800ms round trip to OpenAI, a cache hit returns in under 10ms.

Where I'd be transparent: if you had a use case requiring sub-10ms total response time — like real-time bidding — then yes, a gateway adds overhead that matters. For LLM inference workloads, it doesn't."

**What separates a strong answer:** Candidate quantifies the latency, contextualises it against model inference time, mentions co-location as a mitigation, and is honest about the edge case where it does matter.

**Weak answer:** Candidate says "it's negligible" with no numbers and no acknowledgement of the tradeoff.

---

## Section 5: Behavioural

---

### B1. Technical pushback without an SE

**Question:**
> "Tell me about a time a customer's technical team pushed back hard on your recommendation. What happened?"

**What to listen for:** A specific example where the candidate stood their ground using data or documentation, or knew the right moment to bring in a product expert rather than guessing.

**Red flag:** The candidate either caved immediately or doubled down without evidence.

**Follow-up:** "What would you have done differently?"

---

### B2. Near-lost renewal

**Question:**
> "Describe a renewal you nearly lost and what you did to save it."

**What to listen for:** Early warning signals they noticed (usage drop, champion departure, competitor evaluation), actions taken before the renewal conversation, and what they learned about their account coverage model.

**Red flag:** The candidate only found out the account was at risk at the renewal meeting.

---

### B3. Translating technical to executive

**Question:**
> "Give me an example of translating a technical feature into a business outcome for a non-technical executive."

**What to listen for:** A specific example with a specific executive audience. Strong candidates describe removing jargon, using the customer's business vocabulary, and leading with the outcome rather than the mechanism.

**Follow-up:** "What did you cut from the technical explanation to make it fit?"

---

### B4. Delivering bad news

**Question:**
> "Tell me about a time you had to tell a customer something they didn't want to hear."

**What to listen for:** Proactive communication (told the customer before they found out themselves), a clear explanation of cause and fix, and ownership without blame-shifting to product or engineering.

**Red flag:** Candidate waited to be asked, or framed the bad news as a product limitation rather than a customer outcome.

---

### B5. Managing multiple enterprise accounts

**Question:**
> "How do you manage five enterprise accounts at different stages simultaneously?"

**What to listen for:** A concrete prioritisation system — not "I'm good at multitasking." Strong candidates describe health scoring, tiering accounts by renewal risk and growth potential, and protecting time for proactive outreach rather than only reacting to inbound issues.

**Follow-up:** "Which account do you deprioritise when everything is on fire at once, and how do you make that call?"

---

*Guide prepared for ApexTrans × Kong AI Gateway Technical CSM interview — September 2026*

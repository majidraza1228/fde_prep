# Asset Management & Hedge Funds — FDE & PM Prep

**Why this vertical matters:**
- 55% of asset managers had AI in at least one investment process by 2026
- Over 70% of hedge funds projected to rely on agentic AI by end of 2026
- Mid-size funds ($500M–$5B AUM) spending $400K–$1.5M on AI in year one
- Data-heavy, compliance-heavy, trust-critical — perfect FDE/PM interview scenario

---

## Industry Overview

### How Asset Management Works

| Role | What they do | Their AI pain |
|---|---|---|
| Portfolio Manager | Decides what to buy/sell | Drowning in research, slow signal processing |
| Analyst | Researches companies, sectors | Manual summarization, earnings call review |
| Risk Manager | Monitors portfolio exposure | Slow reporting, reactive not proactive |
| Compliance Officer | Ensures regulatory adherence | Manual regulatory filing, audit trail gaps |
| Client Relations | Reports to investors | Manual report generation, slow personalization |

### Types of Funds

| Type | Focus | AI priority |
|---|---|---|
| Quant hedge fund | Algorithmic signals, high frequency | ML models, signal generation, backtesting |
| Fundamental hedge fund | Deep research, long/short equity | LLM for research synthesis, earnings analysis |
| Long-only asset manager | Index, active equity | Client reporting, portfolio monitoring |
| Multi-strategy fund | Mix of all above | Orchestration across desks |
| Family office | Wealth management | Document Q&A, personalized reporting |

---

## Key AI Use Cases (What You'd Deploy as FDE / What You'd Build as PM)

### 1. Investment Research Synthesis
**What it does:** Agent reads 10-Ks, earnings transcripts, analyst reports, news → generates structured research summary with key risks, catalysts, and sentiment.

**FDE deployment:** RAG over internal research DB + Claude → analyst Q&A interface
**PM product:** Research copilot with citation, version control, and compliance audit trail
**Constraint:** Data can't leave the fund's VPC — use private deployment or open source model

```
Data sources → Ingestion → Vector DB → RAG Agent → Structured output
10-K filings     PDF parser   pgvector     Claude       Research memo
Earnings calls   Whisper STT  Pinecone     + tools      with citations
Analyst notes    text clean
```

### 2. Regulatory Filing Automation
**What it does:** Auto-generate Form PF, Form ADV, AIFMD Annex IV by extracting required data from portfolio management systems. Saves 60–80% of manual effort.

**FDE deployment:** Connect to existing PMS (Bloomberg, Advent) → agent extracts fields → drafts filing → compliance officer reviews
**PM product:** Filing workflow with human-in-the-loop review, version history, regulator submission tracking
**Constraint:** Must have full audit trail — every AI-generated field must show source

### 3. Portfolio Monitoring & Risk Alerts
**What it does:** Monitor positions in real time → flag concentration risk, sector exposure breach, drawdown threshold → generate plain-English explanation for PM.

**FDE deployment:** Stream market data → agent evaluates against fund rules → push alerts to Slack/email
**PM product:** Alert dashboard with explainability — "why did this trigger?" must be answerable
**Constraint:** Latency matters — alerts need to fire in seconds, not minutes

### 4. Client Reporting
**What it does:** Generate personalized investor letters, quarterly reports, fund factsheets based on actual portfolio data.

**FDE deployment:** Portfolio data → Claude → templated report → compliance review gate → send
**PM product:** Report builder with brand control, compliance gate, multi-fund support
**Constraint:** Every number must be verifiable — hallucination here causes legal exposure

### 5. Earnings Call Analysis
**What it does:** Transcribe earnings calls → extract management tone, guidance changes, key risks → compare to prior quarters.

**FDE deployment:** Whisper STT → Claude analysis → structured JSON output → push to research DB
**PM product:** Earnings intelligence feed with sentiment trend, delta from last quarter, alert on surprises

### 6. Compliance Monitoring (AI Co-pilot)
**What it does:** Flag communications (email, chat) that may violate trading restrictions, material non-public information rules, or conduct policies.

**FDE deployment:** Connect to email/comms systems → agent monitors in near-real-time → escalate to compliance team
**PM product:** Compliance dashboard with case management, escalation workflow, regulator-ready audit log

---

## Key Constraints — What Every FDE Must Know

### 1. Data Cannot Leave the Fund's Infrastructure

Most hedge funds and asset managers will NOT send data to external APIs. Their reasons:
- Proprietary trading signals (alpha leakage)
- Client data privacy (PII, portfolio holdings)
- Regulatory requirements (GDPR, SEC, FCA)

**FDE solution options:**

| Option | How | Tradeoff |
|---|---|---|
| Open source model on-prem | Llama 3 / Mistral on their GPU | Loses some quality vs Claude |
| AWS Bedrock / Azure OpenAI | Private endpoint in their VPC | Good quality, data stays in their cloud |
| Anthropic Enterprise | Private deployment agreement | Best quality, higher cost |
| Fine-tuned open source | Llama fine-tuned on their domain | Quality + privacy, expensive to set up |

### 2. Every Output Must Be Auditable

Regulators (SEC, FCA, EU AI Act) require funds to explain AI-generated decisions.

**FDE requirement:** Every agent output must log:
- Which model and version was used
- What source data it referenced
- What the output was and when
- Who reviewed it (human-in-the-loop)

```python
def log_ai_decision(output, sources, model, reviewer_id):
    audit_record = {
        "timestamp": datetime.utcnow().isoformat(),
        "model": model,
        "model_version": get_model_version(model),
        "sources_used": sources,           # which docs were retrieved
        "output": output,
        "output_hash": hash(output),       # tamper detection
        "reviewed_by": reviewer_id,
        "review_timestamp": None           # filled when human approves
    }
    write_to_audit_db(audit_record)
```

### 3. Human-in-the-Loop is Non-Negotiable

Only 5% of asset managers give AI autonomous authority. 95% require human review before action.

**FDE pattern:** AI drafts → human reviews → human approves → system executes
Never deploy an agent that takes autonomous action on portfolios or client communications.

### 4. Regulatory Landscape

| Regulation | What it requires | PM/FDE impact |
|---|---|---|
| EU AI Act (enforced Aug 2026) | High-risk AI must be documented, monitored, human-overseen | Audit trail, explainability features are mandatory |
| SEC AI guidance | Disclose how AI is used in investment decisions | Product must support disclosure reporting |
| FCA (UK) | Firms must understand the AI they use | Can't use a black-box — must explain decisions |
| GDPR | Client data privacy | No client PII in prompts to external APIs |
| MiFID II | Best execution, no conflicts | AI recommendations must be explainable |

---

## Common Customer Objections — FDE Scenarios

### "Our data can't leave our network"
**Answer:** "Understood — we have three options. We can deploy an open source model like Llama 3 directly inside your infrastructure so no data touches an external API. We can use AWS Bedrock or Azure OpenAI which give you a private endpoint inside your existing cloud account. Or if you need Claude's quality specifically, Anthropic has enterprise deployment options. Which of those fits your security posture?"

### "How do we know the AI didn't make up a number in the report?"
**Answer:** "Every output is grounded in retrieved source documents and we log which sources were used. We add citations so compliance can trace any number back to the original filing. Nothing goes to the client without a human review gate."

### "Our compliance team will never approve this"
**Answer:** "Compliance concerns are usually about audit trail and explainability — both of which we build in from day one. We log every model call, every source referenced, every decision made, and every human approval. That's actually more auditable than a manual analyst workflow."

### "The other vendor said they could fine-tune on our data"
**Answer:** "Fine-tuning changes how the model behaves, not what facts it knows. If your goal is for the AI to answer questions about your specific holdings and research — that's a retrieval problem, not a fine-tuning problem. RAG gives you that with citations and the ability to update as your data changes. Fine-tuning is worth considering later if you want to change the model's writing style or domain reasoning — but it's not the right first step."

### "What if the model hallucinates a portfolio value?"
**Answer:** "That's the highest-risk failure mode in this vertical, so we design against it specifically. All numerical outputs are grounded — the model pulls the number from your data and cites the source rather than generating it. We also add a validation layer that checks AI-generated numbers against your source systems before they go into any client-facing document."

---

## FDE Mock Scenarios — Asset Management

**Scenario 1 (deploy):**
> "A $2B long/short equity hedge fund wants an AI that can read earnings call transcripts and tell analysts which companies have changed their guidance tone. They use Bloomberg Terminal and have an internal research database. Data cannot leave their network."

Build this:
```
1. Transcription: Whisper STT on earnings call audio → raw text
2. Ingestion: chunk + embed → store in pgvector (on-prem)
3. Agent: RAG over prior quarters + current transcript → structured output
4. Output: JSON {tone_delta, guidance_change, key_quotes, risk_flags}
5. Delivery: push to their internal research portal via API
6. Audit: log model, sources, output, timestamp to compliance DB
```

**Scenario 2 (blocker):**
> "You're on-site. Their IT team won't give you access to Bloomberg data feeds. The project is stalled."

Answer: "I'd work around the immediate blocker by starting with the data we do have access to — their internal research PDFs and manually uploaded transcripts. Simultaneously I'd get the business stakeholder (the PM or CIO) to escalate the Bloomberg access request, framing it as blocking a project they've already approved budget for. I'd also check if Bloomberg has an API tier that IT has already approved for other vendors — we might be able to get access faster through an existing contract."

---

## PM Mock Scenarios — Asset Management

**Product sense question:**
> "Design an AI research copilot for a fundamental hedge fund analyst. What's your north star metric?"

Answer structure:
```
Clarify: long-only or long/short? how many analysts? what's their current research workflow?
User: fundamental analyst, covers 20-30 stocks, spends 4-6 hrs/day on manual research
Pain: too much to read, slow synthesis, inconsistent format across reports
North star: Hours saved per analyst per week on research synthesis
Guardrail: Research quality score (analyst-rated, sampled weekly)
Features: earnings transcript analysis, 10-K Q&A, sentiment delta vs prior quarter, citation required
Risk: hallucinated numbers in research memos → legal/compliance exposure
Mitigation: all numbers must be grounded + cited, human review gate before distribution
```

**Strategy question:**
> "Should we build our own vector database for financial document search, or use Pinecone?"

Answer:
```
Build if: search quality is core differentiation, need custom financial document parsing
Buy (Pinecone) if: search is infrastructure, not product — focus eng on the AI layer
Recommendation: Buy Pinecone now, own the ingestion pipeline and chunking strategy
Why: chunking strategy for financial docs (10-Ks, footnotes, tables) IS differentiation
The vector DB is a commodity — how you parse and chunk financial documents is not
```

---

## Technical Architecture — Financial AI Stack

```
┌─────────────────────────────────────────────────────┐
│                  Customer VPC / On-Prem              │
│                                                      │
│  Data Sources          Ingestion          AI Layer   │
│  ┌──────────┐         ┌─────────┐       ┌────────┐  │
│  │Bloomberg │────────▶│ Parser  │──────▶│Vector  │  │
│  │Internal  │         │Chunker  │       │  DB    │  │
│  │PDFs/CSVs │         │Embedder │       │(pgvec) │  │
│  └──────────┘         └─────────┘       └───┬────┘  │
│                                             │        │
│  ┌──────────────────────────────────────────▼──────┐ │
│  │              Agent / Harness                    │ │
│  │  RAG retrieve → LLM call → structured output   │ │
│  │  (Claude private / Llama on-prem / Bedrock)     │ │
│  └──────────────────────────────────────────┬──────┘ │
│                                             │        │
│  ┌──────────────┐    ┌─────────────────────▼──────┐ │
│  │  Audit Log   │◀───│   Human Review Gate        │ │
│  │  (tamper-    │    │   compliance approves       │ │
│  │   evident)   │    │   before output ships       │ │
│  └──────────────┘    └────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

---

## Key Numbers to Drop in Interviews

| Fact | Source |
|---|---|
| 55% of asset managers have AI in at least one investment process (2026) | Mercer survey |
| Only 5% give AI autonomous authority | Mercer survey |
| Regulatory filing automation saves 60–80% of manual effort | Industry benchmark |
| Bridgewater AI co-pilot cut compliance review time by 70% | Public reporting |
| Mid-size fund AI year-one cost: $400K–$1.5M | Industry estimate |
| 4 of top 5 AI risks in finance are data-related | Bank of England/FCA survey |
| EU AI Act full enforcement: August 2, 2026 | Regulation |

---

## Reference Resources

| Resource | Why |
|---|---|
| [AI for Hedge Funds: 2026 Guide — Tommaso Maria Ricci](https://www.tommasomariaricci.com/blog/ai-for-hedge-funds) | Cost, tools, alpha playbook — comprehensive |
| [25 AI Use Cases for Hedge Funds — Finantrix](https://www.finantrix.com/articles/25-ai-enabled-automation-use-cases-for-hedge-funds) | Full use case list to know cold |
| [AI Agents in Hedge Funds — Digiqt](https://digiqt.com/blog/ai-agents-in-hedge-funds/) | Agent architecture patterns for finance |
| [Securing Agentic AI in Hedge Funds — HedgeThink](https://www.hedgethink.com/securing-the-agentic-ai-infrastructure-of-2026-hedge-funds/) | Security and compliance requirements |
| [AI for Asset Management — Pascal AI Labs](https://pascalailabs.com/guides/ai-for-asset-management) | Complete deployment guide |

# System Design for Agentic Systems
**Role:** AI Engineer / Forward Deployed Engineer  
**Area:** Agentic system design — architecture, orchestration, tools, failure modes, cost, security

---

## How to Answer Any Agentic System Design Question

Never jump straight to architecture. This is the #1 signal interviewers look for:

```
1. Clarify requirements
   - Data sensitivity? (can it leave their VPC?)
   - Latency? (real-time vs batch)
   - Volume? (10 users vs 10,000 portfolios/night)
   - Cost budget? ($0.04/run vs $0.40/run)
   - Who audits actions? (compliance requirement?)

2. Draw the data flow
   [Source] → [Ingestion] → [Agent] → [Output]

3. Name failure modes — what breaks and how you handle it

4. Add observability from the start — never bolt it on later

5. Mention cost — model tier, caching, batching opportunity
```

---

## Core Concept: What is an Agent?

An agent is a loop: **LLM decides → tool executes → result fed back → LLM decides again.**

```python
import anthropic, os

client = anthropic.Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])

system = [{
    "type": "text",
    "text": "You are a wealth management analyst...",
    "cache_control": {"type": "ephemeral"}   # cache static system prompt
}]

def run_agent(user_input, tools):
    messages = [{"role": "user", "content": user_input}]

    for i in range(10):                          # GUARD — never while True
        response = client.messages.create(
            model="claude-sonnet-4-6",
            system=system,
            tools=tools,
            max_tokens=1024,
            messages=messages
        )

        if response.stop_reason == "end_turn":   # done
            return response.content[0].text

        if response.stop_reason == "tool_use":   # wants a tool
            tool = response.content[0]
            result = execute_tool(tool.name, tool.input)

            messages.append({"role": "assistant", "content": response.content})
            messages.append({
                "role": "user",
                "content": [{
                    "type": "tool_result",
                    "tool_use_id": tool.id,      # CRITICAL — 400 error if wrong
                    "content": str(result)
                }]
            })

        elif response.stop_reason == "max_tokens":
            raise Exception("Response truncated — increase max_tokens")

    raise Exception("Max iterations hit — loop guard triggered")
```

**The 3 non-negotiables:**
1. `for i in range(N)` — never `while True`
2. `tool_use_id: tool.id` — must match exactly or you get a 400
3. Return errors as tool results — never raise from execute_tool

---

## Tool Design

```python
tools = [
    {
        "name": "get_market_data",
        "description": "Fetch current price and 30-day performance for a stock ticker",
        "input_schema": {
            "type": "object",
            "properties": {
                "ticker": {"type": "string", "description": "Stock symbol e.g. AAPL"}
            },
            "required": ["ticker"]
        }
    },
    {
        "name": "read_portfolio_csv",
        "description": "Read a client portfolio from a CSV file",
        "input_schema": {
            "type": "object",
            "properties": {
                "file_path": {"type": "string"}
            },
            "required": ["file_path"]
        }
    }
]
```

**Tool design rules:**
- Narrow tools beat broad tools — one tool, one job
- Description must be specific enough for the LLM to choose correctly
- Every tool needs a clear error contract

---

## Tool Error Handling

Your code handles execution errors. The agent handles semantic errors.

```python
ALLOWED_TOOLS = {"read_portfolio", "get_market_data", "send_email"}

def execute_tool(tool_name, tool_input):
    if tool_name not in ALLOWED_TOOLS:           # whitelist check
        return {"error": f"Tool {tool_name} not permitted"}

    if tool_name == "get_market_data":
        try:
            return fetch_market_api(tool_input["ticker"])
        except Exception as e:
            return {"error": str(e)}             # return, never raise
```

---

## Orchestration Patterns

### Pattern 1: Single Agent

```
User → Agent → [Tool A, Tool B, Tool C] → Response
```

Use when: one model handles the full task, tools are independent.

### Pattern 2: Router + Specialist (Multi-Agent)

```
User → Router Agent (Haiku — cheap)
              ↓
   [Legal Agent | Finance Agent | Summary Agent]  (Sonnet/Opus)
              ↓
        Final Response
```

Use when: tasks need different expertise, prompts, or model tiers.  
Cost rule: router uses Haiku, specialists use Sonnet or Opus.

### Pattern 3: Parallel Tool Execution

```python
import asyncio

async def run_parallel_tools(tickers):
    tasks = [fetch_market_data(t) for t in tickers]
    results = await asyncio.gather(*tasks)
    return dict(zip(tickers, results))
```

Use when: multiple tool calls are independent — run together, not sequentially.

### Pattern 4: Human-in-the-Loop

```python
def execute_tool(tool_name, tool_input):
    if tool_name in IRREVERSIBLE_TOOLS:          # send_email, delete, write
        confirmed = input(f"Confirm: {tool_name}({tool_input})? [y/n]: ")
        if confirmed != "y":
            return {"error": "User rejected action"}
    return run_tool(tool_name, tool_input)
```

Use when: action is irreversible (send email, delete record, write to DB).

---

## Batch Processing

```python
def batch_process(portfolios):
    for portfolio in portfolios:
        try:                                     # isolate each item
            file_path = portfolio["file"]
            advisor_email = portfolio["email"]

            market_data = read_csv(file_path)
            recommendation = run_agent(
                f"Analyze this portfolio: {market_data}"
            )
            send_email(advisor_email, recommendation)

        except Exception as e:
            print(f"Failed: {file_path} — {e}")  # log and keep going
```

**Rule:** One broken item must never crash the rest. Always wrap in try/except.

---

## State Management

| Approach | When to use |
|---|---|
| In-context (messages list) | Short sessions, few turns |
| External KV store (Redis) | Long sessions, need to resume later |
| Vector DB (pgvector, Pinecone) | Semantic search over past interactions |
| Summarization | Context growing too large — summarize old turns |

```python
def trim_context(messages, keep_last_n=6):
    if len(messages) > keep_last_n:
        summary = summarize(messages[:-keep_last_n])
        return [{"role": "user", "content": f"Previous context: {summary}"}] + messages[-keep_last_n:]
    return messages
```

---

## RAG Pattern (Retrieval-Augmented Generation)

Used when: agent needs to answer questions over a large document corpus.

```
PDF/Docs → Chunker → Embeddings → Vector DB
                                      ↓
User Query → Embedding → top-k retrieval → Claude + retrieved chunks → Answer + Citations
```

```python
def rag_query(user_question, vector_db, top_k=5):
    query_embedding = embed(user_question)
    chunks = vector_db.search(query_embedding, top_k=top_k)

    context = "\n\n".join([c["text"] for c in chunks])
    response = client.messages.create(
        model="claude-sonnet-4-6",
        messages=[{
            "role": "user",
            "content": f"Answer using only the context below.\n\n{context}\n\nQuestion: {user_question}"
        }],
        max_tokens=1024
    )
    return response.content[0].text
```

**Failure modes in RAG:**
- Retrieval misses relevant chunks → tune chunk size, add reranker
- Hallucination → require citation, validate it exists in source
- PII leakage → access control at retrieval layer (filter by user permissions)

---

## FDE System Design Framework

```
[Data Source] → [Ingestion Layer] → [Agent] → [Output Layer]
     CSV              Python            Claude        Email / DB / Dashboard
     API           file reader         + tools
     DB            SQL query
```

For enterprise customers, always ask:
- Can data leave their VPC? → If no: Bedrock, Azure OpenAI, or private link
- What's the latency requirement? → Real-time vs batch changes the whole architecture
- Who audits actions? → Every tool call needs a log

---

## Guardrails Checklist

| Guardrail | Implementation |
|---|---|
| Infinite loop prevention | `for i in range(max_iterations)` |
| Tool whitelist | Check `tool_name in ALLOWED_TOOLS` before execution |
| Human-in-the-loop | Confirm before irreversible actions |
| Output validation | Validate structured output schema before acting |
| Context size alert | Log warning when `input_tokens > 80%` of model limit |
| Timeout | Wrap agent run in a timeout — don't let it hang |

---

## Common Interview Questions + Answer Frames

### "Design a document Q&A agent for a law firm"

```
Clarify:
  - Data stays in their VPC?
  - Source doc format (PDF, Word)?
  - Citation required in answers?
  - How many docs? How many users?

Architecture:
  PDF → chunker → embeddings → vector DB (pgvector)
  User query → embedding → top-k retrieval → Claude with chunks
  Claude returns answer + source citations

Failure modes:
  - Retrieval misses → tune chunk size, add reranker
  - Hallucination → require citation, validate it exists
  - PII leakage → access control at retrieval layer

Cost:
  - Cache system prompt (static legal instructions)
  - Haiku for retrieval ranking, Sonnet for final answer
```

### "The agent loops infinitely — how do you fix it?"

```
1. Immediate fix: add for i in range(N) guard
2. Check logs: which tool is it calling repeatedly?
3. Is the tool returning an error? Agent may be retrying a failing tool
4. Is the tool result ambiguous? Agent doesn't know when to stop
5. Add explicit stop condition in system prompt
6. Alert if any run hits > 5 iterations
```

### "Design an agent where data can't leave the customer's VPC"

```
Options:
  1. Deploy Claude via Bedrock (AWS) — data stays in their account
  2. Azure OpenAI with private endpoint — data stays in their Azure region
  3. Self-hosted open source model (Llama) — fully on-prem

Architecture stays the same — only the API endpoint changes.
Secrets: use their secrets manager (AWS Secrets Manager, Azure Key Vault)
Audit: every API call logged to their SIEM
```

### "Our agent costs $0.40/run — get it to $0.04"

```
1. Measure — log tokens in/out per step, find the expensive steps
2. Cache system prompt — saves ~90% on repeated static instructions
3. Tier down — is Opus needed? Try Sonnet. Sonnet? Try Haiku.
4. Trim context — pass last 3 turns, not full history
5. Batch — if not real-time, use Batch API (50% off)
6. Compress tool results — summarize before passing back to model
7. Cap output — set max_tokens, use structured outputs not free text
```

### "Design a multi-agent system for a financial firm"

```
Clarify:
  - What tasks? (portfolio analysis, compliance check, report generation)
  - Real-time or batch?
  - Human approval needed for any actions?

Architecture:
  Router Agent (Haiku) → classifies task type
    → Portfolio Agent (Sonnet) — reads CSV, calls market API
    → Compliance Agent (Opus) — checks regulatory rules
    → Report Agent (Sonnet) — generates PDF summary

Guardrails:
  - Each agent has its own tool whitelist
  - Compliance agent output required before any trade execution
  - All tool calls logged with agent_id, timestamp, input, output

Cost:
  - Router is Haiku (cheap)
  - Cache shared system context across all agents
  - Batch overnight reports
```

---

## Key Rules to Memorize

| Rule | Why |
|---|---|
| `for i in range(N)` not `while True` | Prevents infinite loops and runaway costs |
| Wrap batch items in try/except | One failure must not stop the whole batch |
| Return errors as tool results, don't raise | Agent decides how to handle, not your code |
| Narrow tools beat broad tools | Reduces ambiguity in tool selection |
| Clarify before designing | #1 FDE signal — never design blind |
| Observability from day 1 | Silent failures are the hardest to debug |
| `tool_use_id` must match `tool.id` | Wrong ID = 400 error |
| Router = Haiku, specialists = Sonnet/Opus | Cost efficiency in multi-agent systems |

---

## The 3 stop_reasons

| stop_reason | Meaning | What to do |
|---|---|---|
| `end_turn` | Finished normally | Return `response.content[0].text` |
| `tool_use` | Wants to call a tool | Execute tool, append result, loop |
| `max_tokens` | Output cut off | Raise exception or increase max_tokens |

---

## Intro Question Framework

**"Tell me about a project where you built something with an LLM end-to-end."**

```
1. Context     — what was the problem, who was the customer/user
2. Architecture — what you built (tools, models, data flow — name them)
3. What broke  — a real failure (cost spike, tool loop, hallucination, latency)
4. How you fixed it — specific technical decision
5. Result      — metric or outcome (cost reduction %, latency, accuracy)
```

**What kills answers:**
- "It worked great" — no one believes this
- Vague stack — name the model, the SDK, the tools
- No metrics

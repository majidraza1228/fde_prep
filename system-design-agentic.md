# System Design for Agentic Systems
**Role:** Forward Deployed Engineer  
**Area:** System design — agent architecture, orchestration, tools, failure modes

---

## How to Answer a System Design Question

Always open with clarifying questions — this is the #1 FDE signal:

```
1. Clarify requirements (data sensitivity? latency? cost budget? volume?)
2. Draw the data flow (user → orchestrator → tools → response)
3. Name failure modes and how you'd handle each
4. Add observability from the start (don't bolt it on)
5. Mention cost — what model tier, caching, batching
```

Never jump straight to architecture. Interviewers mark you down for designing without knowing the constraints.

---

## What is an Agent?

An agent is a loop: **LLM decides → tool executes → result fed back → LLM decides again.**

```python
def run_agent(user_input, tools, max_iterations=10):
    messages = [{"role": "user", "content": user_input}]
    
    for i in range(max_iterations):
        response = client.messages.create(
            model="claude-sonnet-4-6",
            tools=tools,
            messages=messages
        )
        
        if response.stop_reason == "end_turn":
            return response.content[0].text
        
        if response.stop_reason == "tool_use":
            tool_result = execute_tool(response)
            messages.append({"role": "assistant", "content": response.content})
            messages.append({"role": "user", "content": tool_result})
        
    raise Exception("Max iterations hit — agent loop guard triggered")
```

**Critical rule:** Always use `for i in range(N)` NOT `while True`. Without a hard cap, a looping agent runs forever and burns your API budget.

---

## Tool Definition

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
- Every tool needs a schema and a clear error contract
- Description must be specific enough for the LLM to choose correctly

---

## Tool Error Handling

When an external API is down, **never crash the agent**. Return the error as a tool result so the agent can decide what to do:

```python
def execute_tool(tool_name, tool_input):
    if tool_name == "get_market_data":
        try:
            return fetch_market_api(tool_input["ticker"])
        except APIDownError as e:
            return {"error": f"Market API unavailable: {e}"}
```

**Rule:** Your code handles execution errors. The agent handles semantic errors. Never raise from a tool — return the error as data.

---

## Batch Processing (High Volume)

```python
def batch_process(portfolios):
    for portfolio in portfolios:
        try:
            file_path = portfolio["file"]
            advisor_email = portfolio["email"]
            
            market_data = read_csv(file_path)
            recommendation = run_agent(
                f"Analyze this portfolio and recommend rebalancing: {market_data}"
            )
            send_email(advisor_email, recommendation)
            
        except Exception as e:
            print(f"Failed on {file_path}: {e}")
```

**Critical rule:** Always wrap each item in try/except. One broken CSV should never crash 9,999 others.

---

## Orchestration Patterns

### Single Agent (most common)

```
User → Agent → [Tool A, Tool B, Tool C] → Response
```

Use when: one model can handle the full task, tools are independent.

### Router + Specialist (multi-agent)

```
User → Router Agent → classify task
                    ↓
         [Legal Agent | Finance Agent | Summary Agent]
                    ↓
              Final Response
```

Use when: tasks require different expertise, prompts, or models. Router uses Haiku (cheap), specialists use Sonnet/Opus.

### Parallel Tool Execution

```python
import asyncio

async def run_parallel_tools(tickers):
    tasks = [fetch_market_data(t) for t in tickers]
    results = await asyncio.gather(*tasks)
    return dict(zip(tickers, results))
```

Use when: multiple tool calls are independent — run them together, not sequentially.

---

## State Management

| Approach | When to use |
|---|---|
| In-context (messages list) | Short conversations, few turns |
| External KV store (Redis) | Long sessions, need to resume later |
| Vector DB (Pinecone, pgvector) | Semantic search over past interactions |
| Summarization | When context grows too large — summarize old turns |

```python
def trim_context(messages, keep_last_n=6):
    if len(messages) > keep_last_n:
        summary = summarize(messages[:-keep_last_n])
        return [{"role": "user", "content": f"Previous context: {summary}"}] + messages[-keep_last_n:]
    return messages
```

---

## FDE System Design Pattern

```
[Data Source] → [Ingestion Layer] → [Agent] → [Output Layer]
     CSV              Python            Claude        Email / DB / Dashboard
     API           file reader         + tools
     DB            SQL query
```

For enterprise customers, always ask:
- Can data leave their VPC? → If no, deploy on-prem or in their cloud account
- What's the latency requirement? → Real-time vs batch changes the whole architecture
- Who audits the actions? → Every tool call needs a log

---

## Guardrails Checklist

| Guardrail | Implementation |
|---|---|
| Infinite loop prevention | `for i in range(max_iterations)` |
| Tool whitelist | Check `tool_name in ALLOWED_TOOLS` before execution |
| Human-in-the-loop | Require confirmation for irreversible actions (send email, delete, write) |
| Output validation | Validate structured output schema before acting on it |
| Context size alert | Log warning when `input_tokens > 80%` of model context limit |
| Timeout | Wrap agent run in a timeout — don't let it hang indefinitely |

---

## Common Interview Questions + Answer Frames

### "Design a document Q&A agent for a law firm"

```
Clarify: data stays in their VPC? source docs format? citation required?

Architecture:
  PDF → chunker → embeddings → vector DB (pgvector)
  User query → embedding → top-k retrieval → Claude with retrieved chunks
  Claude returns answer + source citations

Failure modes:
  - Retrieval misses relevant chunks → tune chunk size, add reranker
  - Hallucination → require citation, validate citation exists in source
  - PII leakage → access control at retrieval layer (filter by user permissions)

Cost:
  - Cache system prompt (static legal instructions)
  - Use Haiku for retrieval ranking, Sonnet for final answer
```

### "The agent loops infinitely. How do you fix it?"

```
1. Add for i in range(N) guard — immediate fix
2. Check logs: which tool is it calling repeatedly?
3. Is the tool returning an error? Agent may be retrying a failing tool
4. Is the tool result ambiguous? Agent may not know when to stop
5. Add explicit stop condition in system prompt: "When you have enough data, call submit_answer"
6. Add iteration count to logs — alert if any run hits > 5 iterations
```

### "Design an agent for a customer whose data can't leave their VPC"

```
Options:
  1. Deploy Claude on their cloud (Bedrock / Azure OpenAI) — data stays in their account
  2. Self-hosted open source model (Llama) — fully on-prem
  3. Private link / VPC peering to Anthropic — data in transit only, never stored

Architecture stays the same — only the API endpoint changes.
Secrets: use their secrets manager (AWS Secrets Manager, Azure Key Vault), not env vars.
Audit: every API call logged to their SIEM.
```

### "Our agent costs $0.40 per run. Get it to $0.04."

```
1. Measure — log tokens in/out per step, find expensive steps
2. Cache system prompt — saves ~90% on repeated static instructions
3. Tier down — is Opus needed? Try Sonnet. Is Sonnet needed? Try Haiku.
4. Trim context — pass last 3 turns, not full history
5. Batch — if not real-time, use Batch API (50% off)
6. Compress tool results — summarize before passing back to model
7. Cap output — set max_tokens, use structured outputs not free text
```

---

## Key Rules to Memorize

| Rule | Why |
|---|---|
| Use `for i in range(N)` not `while True` | Prevents infinite loops and runaway costs |
| Wrap batch items in try/except | One failure must not stop the whole batch |
| Return errors as tool results, don't raise | Agent decides how to handle, not your code |
| Narrow tools beat broad tools | Reduces ambiguity in tool selection |
| Clarify requirements before designing | #1 FDE signal — don't design blind |
| Add observability from day 1 | Silent failures are the hardest to debug |

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

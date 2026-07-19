# Prompt Caching — Deep Dive

## What Is Prompt Caching?

Every time you call Claude, you pay for all input tokens — system prompt, tool definitions, context. If those don't change between calls, you're paying repeatedly.

Prompt caching stores a snapshot of your input. Cache hits cost ~10% of normal input price.

---

## How to Enable It

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    system=[
        {
            "type": "text",
            "text": "You are a wealth management analyst...",
            "cache_control": {"type": "ephemeral"}
        }
    ],
    messages=messages,
    max_tokens=1024
)
```

---

## Rules to Memorize

| Rule | Detail |
|---|---|
| Minimum size | 1024 tokens — below this, no effect |
| Cache lifetime | 5 minutes — resets on each cache hit |
| Cache position | Must be a prefix — cached content before uncached |
| Cache write cost | 25% more than normal input |
| Cache hit cost | ~10% of normal input |

---

## What to Cache vs What Not to Cache

```python
system = [
    {
        "type": "text",
        "text": "You are a senior wealth management analyst... (500+ tokens)",
        "cache_control": {"type": "ephemeral"}   # CACHE — static, same every call
    }
]

tools = [
    {
        "name": "get_market_data",
        "cache_control": {"type": "ephemeral"},  # CACHE — tool definitions don't change
        "description": "...",
        "input_schema": {...}
    }
]

messages = [
    {
        "role": "user",
        "content": f"Analyze this portfolio: {portfolio_data}"  # DO NOT CACHE — changes every call
    }
]
```

---

## Caching Long Documents

Cache large documents so follow-up questions don't re-pay for them:

```python
messages = [
    {
        "role": "user",
        "content": [
            {"type": "text", "text": "Here is the financial report:"},
            {
                "type": "text",
                "text": report_content,
                "cache_control": {"type": "ephemeral"}  # cache the document
            },
            {"type": "text", "text": "What are the top 3 risks?"}  # don't cache the question
        ]
    }
]
```

---

## How to Verify Cache is Working

```python
response = client.messages.create(...)

print(f"Cache write tokens: {response.usage.cache_creation_input_tokens}")
print(f"Cache read tokens:  {response.usage.cache_read_input_tokens}")
print(f"Regular tokens:     {response.usage.input_tokens}")

# First call:  cache_creation_input_tokens > 0  (writing to cache)
# Second call: cache_read_input_tokens > 0       (reading from cache — cheap)
```

---

## Best Practices for Agent Development

```python
# 1. Put cacheable content FIRST — cache is a prefix
system = [
    {"type": "text", "text": STATIC_INSTRUCTIONS, "cache_control": {"type": "ephemeral"}},
    {"type": "text", "text": DYNAMIC_USER_CONTEXT}  # no cache — changes per user
]

# 2. Cache tool definitions
tools = [
    {**tool, "cache_control": {"type": "ephemeral"}}
    for tool in base_tools
]

# 3. Log cache hits to verify savings
def call_claude(messages):
    response = client.messages.create(...)
    
    if response.usage.cache_read_input_tokens > 0:
        logging.info(f"Cache hit — saved {response.usage.cache_read_input_tokens} tokens")
    
    return response

# 4. Keep cache warm — gap > 5 min expires the cache
# For batch jobs: process continuously, avoid long pauses
```

---

## Cost Example — 10,000 Portfolio Runs

| | Without Caching | With Caching |
|---|---|---|
| System prompt | 2,000 tokens × $3/M × 10,000 | Paid once (write) + 10% on hits |
| Per-portfolio | 500 tokens each | 500 tokens each (not cached) |
| **Total** | **~$75** | **~$15 (80% savings)** |

---

## What Interviewers Want to Hear

1. Minimum **1024 tokens** to trigger caching
2. Cache is a **prefix** — static content must come before dynamic
3. **5-minute TTL** — resets on each hit, expires if unused
4. Verify with **`cache_read_input_tokens > 0`**
5. Cache **tools too** — not just system prompts
6. Be able to do **cost math** on the spot

---

## Test Questions (practice these)

1. Your agent runs 10,000 times a day with the same 2,000-token system prompt. How much do you save with caching?
2. Why must cached content come before uncached content?
3. How do you verify a cache hit in code?
4. What is the minimum token count to enable caching?
5. Your cache keeps missing after 6 minutes. Why?

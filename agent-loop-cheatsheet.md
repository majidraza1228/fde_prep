# Agent Loop Cheatsheet

Write this from memory every day until the interview.

## Complete run_agent Function

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])

system = [
    {
        "type": "text",
        "text": "You are a wealth management analyst...",
        "cache_control": {"type": "ephemeral"}
    }
]

def run_agent(user_input):
    messages = [{"role": "user", "content": user_input}]

    for i in range(10):
        response = client.messages.create(
            model="claude-sonnet-4-6",
            system=system,
            tools=tools,
            max_tokens=1024,
            messages=messages
        )

        if response.stop_reason == "end_turn":
            return response.content[0].text

        elif response.stop_reason == "tool_use":
            tool = response.content[0]
            result = execute_tool(tool.name, tool.input)

            messages.append({
                "role": "assistant",
                "content": response.content
            })
            messages.append({
                "role": "user",
                "content": [
                    {
                        "type": "tool_result",   # NOT "text" — causes 400 error
                        "tool_use_id": tool.id,  # must match the tool call
                        "content": str(result)
                    }
                ]
            })

        elif response.stop_reason == "max_tokens":
            raise Exception("Response truncated — increase max_tokens")

    raise Exception("Max iterations hit")
```

---

## Things to Get Right Every Time

| Thing | Correct value | Common mistake |
|---|---|---|
| Model name | `claude-sonnet-4-6` | `sonet-6.6`, `claude-sonnet` |
| Iteration guard | `range(10)` | `while True` |
| Cache control | `{"type": "ephemeral"}` | `{"type": "ephemal"}` (typo) |
| Tool result type | `"tool_result"` | `"text"` |
| Tool result role | `"user"` | `"assistant"` |
| tool_use_id | `tool.id` | hardcoded string |
| end_turn return | `response.content[0].text` | `response.content` |

---

## The 3 stop_reasons

| stop_reason | Meaning | Action |
|---|---|---|
| `end_turn` | Finished normally | Return `response.content[0].text` |
| `tool_use` | Wants to call a tool | Execute tool, append result, loop |
| `max_tokens` | Output cut off | Raise exception |

---

## Daily Practice

1. Close this file
2. Open a blank editor
3. Write the full function from memory
4. Compare to this file
5. Repeat until it matches perfectly

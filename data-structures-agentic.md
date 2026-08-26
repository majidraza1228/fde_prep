# Data Structures in Agentic Programming

How developers apply data structures when building real AI agents — for FDE coding interviews at Anthropic, OpenAI, and Palantir.

---

## The Core Insight

FDE coding tests don't ask "implement a binary tree." They give you a real agent problem and expect you to reach for the right data structure naturally — and implement it correctly, from memory, in under 10 minutes.

---

## The 5 Patterns You Must Know

### 1. Queue — Tool Call Processing

**When it appears:** Claude returns multiple tool calls in one response. You buffer and process them in order without losing any.

**Data structure:** `collections.deque` — FIFO order, O(1) enqueue and dequeue.

**Real use:** Iterating `response.content` blocks after `stop_reason: "tool_use"`, building the `tool_results` list to send back.

```python
from collections import deque

class ToolCallBuffer:
    def __init__(self):
        self._queue = deque()

    def enqueue(self, tool_use_block):
        self._queue.append(tool_use_block)

    def process_all(self, execute_fn):
        results = []
        while self._queue:
            tool = self._queue.popleft()
            try:
                output = execute_fn(tool.name, tool.input)
            except Exception:
                try:
                    output = execute_fn(tool.name, tool.input)  # retry once
                    is_error = False
                except Exception as e:
                    output = str(e)
                    is_error = True
            else:
                is_error = False
            results.append({
                "type": "tool_result",
                "tool_use_id": tool.id,
                "content": str(output),
                "is_error": is_error,
            })
        return results

    def has_pending(self):
        return len(self._queue) > 0

    def size(self):
        return len(self._queue)
```

**Interview trap:** Forgetting `"type": "tool_result"` or using `tool.name` instead of `tool.id` in `tool_use_id`.

---

### 2. HashMap — Context and State Tracking

**When it appears:** Agents need to remember things across turns — tool results, session state, conversation history keyed by ID.

**Data structure:** `dict` — O(1) lookup by key.

**Real use:** Mapping `tool_use_id` back to results, caching tool outputs, tracking which tools have already been called.

```python
# Map tool_use_id → result for quick lookup in multi-turn agent
tool_result_cache = {}

for block in response.content:
    if block.type == "tool_use":
        result = execute_tool(block.name, block.input)
        tool_result_cache[block.id] = result
```

**Interview trap:** Using `block.name` as the key instead of `block.id` — tool names can repeat, IDs are unique per call.

---

### 3. Stack — Conversation Turn Management

**When it appears:** Undo/rollback in multi-turn agents, tracking nested tool calls, managing a scratchpad of intermediate reasoning steps.

**Data structure:** `list` with `append` / `pop` — LIFO order, O(1) push and pop.

**Real use:** Rolling back messages if a tool call fails, maintaining an agent scratchpad.

```python
class AgentScratchpad:
    def __init__(self):
        self._stack = []

    def push(self, step):
        self._stack.append(step)

    def pop(self):
        if not self._stack:
            raise ValueError("Scratchpad is empty")
        return self._stack.pop()

    def peek(self):
        if not self._stack:
            raise ValueError("Scratchpad is empty")
        return self._stack[-1]

    def rollback_to(self, checkpoint_index):
        self._stack = self._stack[:checkpoint_index]
```

---

### 4. Heap / Priority Queue — Task Routing

**When it appears:** Multi-agent systems where some tasks are urgent. Always serve the highest-priority request next in O(log n).

**Data structure:** `heapq` — min-heap by default. Use negative priority for max-heap behavior.

**Real use:** Support ticket router, rate-limited API queue, ranked tool call scheduler.

```python
import heapq

class TicketQueue:
    def __init__(self):
        self._heap = []
        self._counter = 0  # tiebreaker: FIFO within same priority

    def add(self, ticket_id, severity, description):
        # severity 1 = critical, 5 = low — lower number = higher priority
        heapq.heappush(self._heap, (severity, self._counter, ticket_id, description))
        self._counter += 1

    def get_next(self):
        if not self._heap:
            raise ValueError("Queue is empty")
        _, _, ticket_id, description = heapq.heappop(self._heap)
        return ticket_id, description

    def peek(self):
        if not self._heap:
            raise ValueError("Queue is empty")
        _, _, ticket_id, description = self._heap[0]
        return ticket_id, description

    def size(self):
        return len(self._heap)
```

**Interview trap:** Forgetting the tiebreaker counter — without it, Python tries to compare the description strings when severity is equal, which may error or sort incorrectly.

---

### 5. Graph / BFS — Dependency Resolution

**When it appears:** Agent workflows where Task B can't run until Task A finishes. DAG-based orchestration (like LangGraph, Prefect, Airflow).

**Data structure:** Adjacency list (`dict` of `list`) + BFS with a `deque`.

**Real use:** Resolving which agent nodes can run in parallel vs which must wait, topological task ordering.

```python
from collections import deque

def resolve_order(tasks, dependencies):
    """
    tasks: list of task names
    dependencies: dict { task: [list of tasks it depends on] }
    returns: execution order as a list, or raises if cycle detected
    """
    in_degree = {t: 0 for t in tasks}
    graph = {t: [] for t in tasks}

    for task, deps in dependencies.items():
        for dep in deps:
            graph[dep].append(task)
            in_degree[task] += 1

    queue = deque([t for t in tasks if in_degree[t] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(order) != len(tasks):
        raise ValueError("Cycle detected — cannot resolve task order")

    return order
```

**Interview trap:** Not detecting cycles — if you return a partial order without raising, the agent hangs silently on the unresolved tasks.

---

## How to Prepare — 3-Week Plan

| Week | Focus | Daily Drill |
|------|-------|-------------|
| 1 | Queue + HashMap | Write `ToolCallBuffer` from scratch — no docs, under 8 min |
| 2 | Stack + Heap | Build `TicketQueue` with correct tiebreaker from scratch |
| 3 | Graph + BFS | Implement `resolve_order` with cycle detection from scratch |

---

## The One Rule

> No docs. No autocomplete. Under 10 minutes. If you can't write it cold, you don't own it yet.

---

## Practice Drills

Run `/code-drill` in this repo to drill these patterns live:

```
/code-drill          ← random data structure scenario
/code-drill agent    ← full agent loop (uses Queue + HashMap together)
```

Claude will give you the scenario, wait for your code, and score every line against the requirements.

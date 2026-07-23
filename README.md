# FDE Interview Prep

Structured preparation for Forward Deployed Engineer interviews at AI-native companies (Anthropic, OpenAI, Scale AI, Palantir, etc.).

The goal is to be able to answer technical questions cold — without scaffolding, without hints — across system design, cost engineering, security, observability, and the Claude API.

Each area is taught first, then tested. Gaps are scored honestly.

---

## Files

| File | Description |
|---|---|
| [fde-mock-interview-session.md](fde-mock-interview-session.md) | Full study guide — code examples, scores, and key rules from each area |
| [system-design-agentic.md](system-design-agentic.md) | System design deep dive — agent loop, orchestration, tools, failure modes, interview Q&A |
| [microsoft-azure-github.md](microsoft-azure-github.md) | Azure OpenAI, Managed Identity, Content Safety, GitHub Copilot, GitHub Actions |
| [agent-loop-cheatsheet.md](agent-loop-cheatsheet.md) | Complete run_agent function — write this from memory daily |
| [prompt-caching.md](prompt-caching.md) | Prompt caching deep dive — rules, best practices, cost examples, test questions |
| [reference-material.md](reference-material.md) | Official docs and links for deeper reading |

---

## Areas Covered

- [x] Area 1: System Design for Agentic Systems
- [x] Area 2: Token Optimization and Cost Engineering
- [x] Area 3: Security — Prompt Injection, Sandboxing, Secrets
- [x] Area 4: Observability and Debugging
- [x] Area 5: Platform-Specific — Claude API
- [x] Area 6: OpenAI Platform (Responses API, Structured Outputs, Batch API)
- [x] Area 7: Microsoft / Azure / GitHub Copilot
- [x] Area 8: Prompt Caching — Deep Dive
- [x] Area 9: Interview Prep Guide (how to answer intro + design questions)
- [ ] Area 10: Kubernetes + AWS for AI Workloads

---

## Daily Practice

Write these from memory every day until the interview:

1. The full `run_agent` function with correct tool result format
2. The 6 things to log on every API call
3. The 5 token cost reduction techniques

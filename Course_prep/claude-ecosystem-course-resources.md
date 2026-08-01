# The Claude Ecosystem for PMs — Learning Resources
### Companion to `claude-ecosystem-course-for-pms.md` and `claude-ecosystem-course-labs.md`

Official docs and primary sources for every topic in the curriculum, organized by class. Where Anthropic hasn't published a dedicated doc, the best available explainer is listed instead and marked accordingly. Docs move fast — if a link 404s, search `platform.claude.com/docs` or `code.claude.com/docs` for the current path.

---

## Class 1 · The Terrain

**Claude.ai surfaces (Projects, Memory, Artifacts, Connectors)**
- [Intro to Claude](https://platform.claude.com/docs/en/intro) — Claude Platform Docs, entry point to the full doc set
- [Anthropic Academy: Claude API Development Guide](https://www.anthropic.com/learn/build-with-claude)

**Models and the intelligence/cost decision**
- [Claude Platform Docs](https://platform.claude.com/docs) — model overview, capabilities, and current pricing tables (check for the latest Haiku/Sonnet/Opus tiers and rates, which change often)

**Extended Thinking**
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic Engineering
- [Claude's "think" tool](https://www.anthropic.com/engineering/claude-think-tool) — Anthropic Engineering

**Multi-agent as the second scaling lever + Research as a live example**
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) — Anthropic Engineering (the orchestrator/subagent architecture behind Research; ~90% reduction in research time on complex queries)
- [Simon Willison's summary/annotated read](https://simonwillison.net/2025/Jun/14/multi-agent-research-system/) — useful second pass if the original is dense

---

## Class 2 · The Building Blocks

**MCP (protocol, not product)**
- [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/getting-started/intro) — official spec site
- [Model Context Protocol on Wikipedia](https://en.wikipedia.org/wiki/Model_Context_Protocol) — good for the "USB-C for AI" framing and adoption timeline
- [MCP joins the Agentic AI Foundation](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/) — Anthropic donated MCP to the Linux Foundation for vendor-neutral governance; relevant to the "protocol not product" argument
- [MCP Registry announcement](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/) — how servers get discovered

**Skills**
- [Agent Skills overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — Claude Platform Docs
- [Using Agent Skills with the API](https://platform.claude.com/docs/en/build-with-claude/skills-guide)
- [Extend Claude with skills](https://code.claude.com/docs/en/skills) — Claude Code–specific skill docs
- [anthropics/skills](https://github.com/anthropics/skills) — GitHub repo of official open-source Skills, good source for Lab 2A templates

**Plugins**
- [Discover and install prebuilt plugins through marketplaces](https://code.claude.com/docs/en/discover-plugins) — Claude Code Docs
- [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) — official plugin directory and marketplace source

**Slash commands / PMSP framework**
- No single official doc covers the PMSP (Prompt/Project/Skill/MCP) decision matrix directly — it's a teaching framework, not an Anthropic artifact. Build the reference card from the Skills and MCP docs above plus the Projects section of platform.claude.com/docs.

---

## Class 3 · The Engine Room

**Claude Code architecture**
- [Claude Code overview](https://code.claude.com/docs/en/overview) — Claude Code Docs, main entry point
- [Claude Code on GitHub](https://github.com/anthropics/claude-code) — source, issues, and release notes

**Long-running agents and failure modes**
- [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) — Anthropic Engineering, the initializer/coding-agent pattern referenced in Class 3
- [Effective harnesses for long-running agents](https://businessdatasolutions.github.io/ai-wiki/sources/2025-11-26-anthropic-effective-harnesses-long-running-agents) — third-party summary if the original engineering post is hard to access
- Key failure modes documented: context loss/coherence drift over long sessions, "context anxiety" (wrapping up prematurely near perceived context limits), one-shotting instead of incremental progress, and bloated tool sets causing ambiguous tool choice — all covered in the harness-design post above

**Channels (Telegram, Discord, iMessage)**
- [Claude Code Channels announcement](https://venturebeat.com/orchestration/anthropic-just-shipped-an-openclaw-killer-called-claude-code-channels) — VentureBeat coverage (no dedicated Anthropic doc page found at time of writing; verify via `code.claude.com/docs` search for "channels")
- Setup detail: Channels run as MCP servers on your machine, bridging Telegram/Discord to your Claude Code session — the official plugin source is in [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)

---

## Class 4 · The Full Deployment

**Cowork**
- [Anthropic Cowork cloud/mobile expansion coverage](https://9to5mac.com/2026/07/07/anthropic-expanding-claude-cowork-to-mobile-and-web-details-here/) — 9to5Mac, covers the move from desktop-only to web + mobile (remote sessions, scheduled tasks without a device online)
- Check `claude.com/cowork` or `platform.claude.com/docs` directly for the current official product doc, since Cowork has shipped fast changes through 2026

**Computer Use**
- [Computer use tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool) — Claude Platform Docs, the core reference (quickstart, best practices, data handling)
- [claude-quickstarts: computer-use-demo](https://github.com/anthropics/claude-quickstarts/tree/main/computer-use-demo) — minimal reference agent loop against a Linux desktop (Docker + X11/VNC)
- [Initial explorations of Anthropic's Computer Use capability](https://simonwillison.net/2024/Oct/22/computer-use/) — good "what it actually looks like" walkthrough for a non-engineering audience

**Claude for Excel and PowerPoint**
- [Anthropic gives Claude shared context across Excel and PowerPoint](https://venturebeat.com/orchestration/anthropic-gives-claude-shared-context-across-microsoft-excel-and-powerpoint) — VentureBeat, covers the shared-context workflow referenced in Class 4
- [Claude for Excel and PowerPoint: Complete Guide (2026)](https://pasqualepillitteri.it/en/news/265/claude-excel-powerpoint-ai-add-ins-guide) — third-party guide with plan-tier availability details

**Enterprise deployment**
- [Admin API](https://platform.claude.com/docs/en/build-with-claude/administration-api) — Claude Platform Docs
- [Analytics APIs](https://platform.claude.com/docs/en/manage-claude/analytics-api) — org-wide engagement, adoption, and cost data
- [Claude Code Analytics API](https://platform.claude.com/docs/en/manage-claude/claude-code-analytics-api) — Claude Code–specific usage data
- [Claude Code and new admin controls for business plans](https://www.anthropic.com/news/claude-code-on-team-and-enterprise) — Anthropic news post on admin controls

---

## General / Cross-Cutting

- [Claude Platform Docs (root)](https://platform.claude.com/docs) — best single starting point for anything not listed above
- [Anthropic Engineering blog](https://www.anthropic.com/engineering) — primary source for all the architecture posts referenced across Classes 1, 3, and 4
- [Model Context Protocol Blog](https://blog.modelcontextprotocol.io/) — MCP-specific announcements and spec changes

## Instructor Reference 

See the "Reference Videos" section in `claude-ecosystem-course-for-pms.md` for confirmed videos and interviews from Jyothi Nookula relevant to this curriculum.

---

## Notes on Currency

This ecosystem ships fast — Cowork alone went from desktop-only to web/mobile within roughly six months, and model pricing/tiers shift regularly. Before each cohort, do a 15-minute pass through `platform.claude.com/docs`, `code.claude.com/docs`, and the [Anthropic Engineering blog](https://www.anthropic.com/engineering) to catch anything that's moved since this list was compiled (August 2026).

# Glossary

One line each. Terms you'll meet across Claude's products and docs.

**Agent** — An AI that pursues a goal by taking actions in a loop (use tool → read result → decide next step), rather than answering once.

**Agent SDK** — Claude Code's harness packaged as a Python/TypeScript library for building your own agents.

**Artifact** — A standalone piece of content (app, page, doc, diagram) Claude builds in its own panel on claude.ai; can be published, shared, and made interactive.

**CLAUDE.md** — A file of standing instructions Claude Code loads at session start; project memory in a text file.

**Claude Design** — Chat-to-canvas visual design surface (mockups, slides, posters) with a real editor and design-system support.

**Claude Tag** — Claude as a Slack teammate you @-mention in channels; works under its own identity with your org's tools.

**Connector** — A claude.ai integration (built on MCP) giving Claude access to an external service like Drive, Gmail, or Linear.

**Cowork** — Anthropic's agentic surface for non-coding knowledge work: describe an outcome, Claude works across many steps (files, apps, research) and returns finished deliverables.

**Context window** — The model's working memory: everything (prompt, files, conversation, tool results) it can see at once, measured in tokens.

**Effort** — A dial for how much reasoning/compute Claude spends on a response (low → max).

**Extended thinking** — Claude reasoning privately before answering; on current models it's adaptive (spent when the problem needs it).

**Hallucination** — Confident output not grounded in fact or context; mitigated by grounding, citations, verification loops, and "say if unsure."

**Hook** — A shell command Claude Code's harness runs automatically at fixed lifecycle points (before/after tools, session start/stop); deterministic, not up to the model.

**MCP (Model Context Protocol)** — The open standard for connecting AI to tools and data; an MCP server exposes a system (GitHub, a database, a browser) so any MCP client, Claude included, can use it.

**Memory** — Claude's cross-conversation recall of facts about you/your projects on claude.ai (and per-project auto memory in Claude Code); editable, not the same as the context window.

**Plan mode** — Claude Code mode where Claude can read and propose but not change anything; explore before you commit.

**Plugin** — An installable bundle of skills, agents, hooks, and MCP config for Claude Code, distributed via marketplaces.

**Project (claude.ai)** — A workspace with its own instructions, knowledge files, and chat history; a standing context for recurring work.

**Prompt caching** — API feature that reuses an unchanged prompt prefix across calls at a fraction of the cost/latency; the economics behind long system prompts and agents.

**Rate limit / usage limit** — Caps on how much you can use in a window (API: requests/tokens per minute; subscriptions: session and weekly hour-based limits).

**RAG (retrieval-augmented generation)** — Fetching relevant documents at ask-time and putting them in context so answers ground in your data.

**Routine / scheduled task** — A saved instruction that runs on a schedule in a fresh cloud (or desktop) session without you.

**Skill** — A folder (SKILL.md + optional references/scripts) of on-demand expertise Claude loads when the task matches; see Module 01.

**Slash command** — An explicit `/name` invocation in Claude Code; today effectively the manual trigger for a skill.

**Subagent** — A separate Claude instance with its own context window, spawned to handle a subtask and report back.

**System prompt** — The standing instructions that frame every request (who Claude is, rules, context); in apps you control it via project/custom instructions.

**Token** — The unit of text models read/write (~¾ of an English word); context windows and API pricing are measured in tokens.

**Tool use / function calling** — The API mechanism letting Claude invoke functions you define (or server-side tools like web search and code execution) and use the results.

**Vision** — Claude's ability to read images (screenshots, photos, charts, PDFs) as input.

**Workbench / Console** — The developer dashboard (platform.claude.com) for API keys, prompt testing, usage, and billing.

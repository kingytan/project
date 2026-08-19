# Module 05 — The API & Agent SDK: Building With Claude

The apps are Claude used; the API is Claude *shipped*: inside your scripts, your products, your company's pipelines. You don't need to be a full-time engineer for this module to matter. Knowing what the platform can do changes what you ask for at work, and the mental models (caching, model selection, agent architectures) make you sharper on every surface.

---

## 1. When the API is the right surface

- The task runs **on a schedule or at volume** (classify 10,000 tickets, nightly report generation).
- Claude becomes **part of a product** other people use.
- You need **exact control**: which model, how much reasoning, what tools, structured output shapes.
- You want **your own agent** with your tools and your rules.

Access starts at the Console (platform.claude.com): create a key, experiment in the Workbench (it even has a prompt improver), then graduate to the SDKs (Python, TypeScript, and most major languages).

## 2. The 30-second anatomy

One endpoint does nearly everything: `POST /v1/messages`. You send a model ID, a system prompt, a list of messages, optional tools; you get back content blocks and usage counts. Everything else (streaming, tool use, caching, thinking) is a feature of that same call. Supporting endpoints: Batches (async bulk), Files (upload once, reference many times), token counting, and model listing.

```python
from anthropic import Anthropic
client = Anthropic()

response = client.messages.create(
    model="claude-opus-5",
    max_tokens=16000,
    system="You are a precise financial analyst.",
    messages=[{"role": "user", "content": "Summarize the attached 10-K risks."}],
)
print(response.content[0].text)
```

## 3. The model lineup (August 2026)

| Model | ID | Context | $/MTok in / out | Use it for |
|-------|----|---------|-----------------|------------|
| Claude Fable 5 | `claude-fable-5` | 1M | $10 / $50 | The hardest reasoning and long-horizon agent work; new Mythos-class tier above Opus |
| Claude Opus 5 | `claude-opus-5` | 1M | $5 / $25 | Default for demanding work: complex code, deep analysis, agents |
| Claude Opus 4.8 / 4.7 | `claude-opus-4-8` / `-4-7` | 1M | $5 / $25 | Prior Opus generations, still supported |
| Claude Sonnet 5 | `claude-sonnet-5` | 1M | $3 / $15 (intro $2 / $10 through 2026-08-31) | The high-volume workhorse: great quality per dollar |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | 1M | $3 / $15 | Previous workhorse |
| Claude Haiku 4.5 | `claude-haiku-4-5` | 200K | $1 / $5 | Speed and scale: classification, extraction, routing, subagents |

(Claude Mythos 5 shares Fable 5's engine without its dual-use safety measures; approved organizations only.) Notes that matter:

- **Thinking is now adaptive.** Current models decide when and how much to reason; on Fable 5 it's always on. The old fixed "thinking budget" is gone. You steer depth with **effort**: `output_config: {effort: "low" | "medium" | "high" | "xhigh" | "max"}`. Effort is the main quality/cost/speed dial on modern models; `xhigh` is the sweet spot for serious agentic work.
- **1M-token context** across the current lineup: entire codebases or document sets in one call.
- **Output up to 128K tokens** (stream anything that large).
- Prices move; check the current pricing page before you budget anything serious.

**The laddering strategy:** don't pick one model, pick per task-tier. Haiku triages and extracts; Sonnet handles the bulk; Opus/Fable handle the 5% that's genuinely hard, at high effort. Pipelines built this way cost a fraction of "Opus for everything" at nearly identical output quality.

## 4. The features that change what you can build

**Streaming.** Tokens as they generate. Table stakes for anything user-facing; also how you avoid timeouts on big outputs.

**Tool use.** You describe functions (name + JSON schema); Claude decides when to call them and with what arguments; you execute and return results; Claude continues. This is the primitive under every agent. Modern additions: parallel tool calls in one turn, `strict: true` for exact schema-valid arguments, and **structured outputs** (`output_config.format`) when you need the *response itself* as guaranteed-valid JSON.

**Server-side tools.** Anthropic runs these for you, no plumbing: **web search**, **web fetch**, and **code execution** (a sandbox where Claude writes and runs Python — which also powers file generation: spreadsheets, decks, PDFs via Agent Skills in the API). Declare them in `tools` and Claude uses them mid-response.

**Prompt caching.** The economics feature. Mark a stable prefix (system prompt, tool definitions, big documents) with `cache_control`, and repeat calls reuse it at ~10% of input price with much lower latency. Rules that bite: caching is **prefix-based** (one changed byte invalidates everything after it), so stable content goes first, volatile content last; verify with `usage.cache_read_input_tokens`. A timestamp in your system prompt silently zeroes your hit rate. Caching is why agents (which resend history every turn) are affordable at all.

**Batches.** Submit up to thousands of requests, get results within a day, pay **50% less**. Anything not latency-sensitive belongs here.

**Files API.** Upload once, reference by ID forever; no re-sending the same 200-page PDF.

**Long-conversation management.** Server-side **compaction** (auto-summarize old history), **context editing** (auto-clear stale tool results), and the **memory tool** (Claude maintains its own notes across sessions). These three are how "agents that run for hours or weeks" actually work.

**MCP connector.** Point a request at MCP servers and Claude uses their tools mid-call: the same integration standard from Modules 03–04, now in your backend.

## 5. Building agents: the four architectures

The decision that actually matters when someone says "let's build an agent." Two questions separate the options: who runs the **loop** (harness), and who runs the **infrastructure**.

| Approach | You write | Who hosts | Choose when |
|----------|-----------|-----------|-------------|
| **Manual loop** (raw Messages API) | The whole `while tool_use` loop | You | Maximum control, unusual control flow |
| **Tool Runner** (built into the SDK) | Just your tool functions | You | Custom-tool agent without loop boilerplate; hooks for approvals/logging (most custom agents) |
| **Managed Agents** (Anthropic-hosted, beta) | A config + your prompts | Anthropic (loop + per-session sandbox with bash/files/code) | Hosted, stateful, versioned agents; scheduled runs; no infra to own |
| **Claude Agent SDK** (Claude Code as a library) | A prompt + options | You | You want Claude Code's batteries (file tools, bash, subagents, hooks, permissions) inside your own app |

Rules of thumb the top 1% apply: **start at the simplest tier that works** (a single call beats a workflow beats an agent; agents are for genuinely open-ended tasks). Reach for Managed Agents when you want zero infra; reach for the Agent SDK when the agent's job looks like "operate on files and repos"; reach for the Tool Runner when the agent's job is "operate my APIs."

## 6. Cost engineering (where experts embarrass amateurs)

1. **Cache the prefix.** Agents and chat apps without caching pay ~10x for input, self-inflicted.
2. **Ladder models.** Haiku → Sonnet → Opus/Fable by difficulty tier, not one model for all.
3. **Tune effort.** `low` for mechanical steps and subagents; `xhigh`/`max` only where correctness is the product.
4. **Batch the batchable.** Instant 50% off anything asynchronous.
5. **Curate context.** Send the relevant 5K tokens, not the convenient 200K; use Files + retrieval instead of paste-everything.
6. **Count before you send.** The token-counting endpoint is free; surprise bills come from unmeasured prompts.

## 7. Practices that keep you out of trouble

- **Stream long outputs; set `max_tokens` generously.** Truncated-by-cap responses are a silent quality killer.
- **Parse tool inputs as JSON, always;** never regex the serialized string.
- **Handle refusals on frontier models:** Fable 5 can return `stop_reason: "refusal"` with a category; check `stop_reason` before reading content (the API offers automatic fallbacks to route such requests to another model).
- **Use the SDK's types, helpers, and typed errors** instead of hand-rolling loops, promises, and string-matched exceptions.
- **Evaluate like an engineer:** keep a small test set of real inputs + expected qualities, and re-run it when you change prompts or models. The Console's eval tools help; a spreadsheet works too.
- **Mind data settings:** retention and training settings are org-level decisions; know yours before shipping customer data through anything.

## 8. Exercises

1. Workbench: take your Module 01 skill's job and run it as a raw API call; use the prompt improver on your prompt and diff the two.
2. Write a 20-line script (Claude Code will happily write it): classify 50 sample emails with Haiku 4.5, total cost printed. Then re-run via the Batch API.
3. Add `cache_control` to a repeated-call script and verify `cache_read_input_tokens > 0`; break it with a timestamp to see the invalidation.
4. Build one Tool Runner agent with two tools (e.g., `get_calendar`, `send_summary` stubs).
5. Estimate seriously: pick one workflow from your job and price it at Haiku/Sonnet/Opus. The number is usually startlingly small; that realization changes what you propose at work.

---

*Next: `06-integrations-and-automation.md` — Claude woven into the tools you already use.*

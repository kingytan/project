# Module 04 — Claude Code Mastery

Claude Code is Claude with hands: an agent harness that reads files, runs commands, edits code, uses git, browses, and keeps working until the job is done. It began as a coding tool; treat it as **the general-purpose agent surface**. The top 1% run research, file wrangling, data work, and automation through it, not just code.

---

## 1. The mental model

Chat surfaces answer; Claude Code **acts**. One loop explains everything: Claude reads your request → plans → calls tools (read, edit, run, search) → reads the results → repeats until done or blocked. Every feature in this module is a way to shape that loop:

- **Context** shapes what it knows (CLAUDE.md, rules, memory, skills).
- **Permissions** shape what it may do.
- **Hooks** bolt deterministic steps onto the loop.
- **Subagents** give the loop more hands and fresh memory.
- **MCP** gives the loop reach into other systems.

Master those five levers and you have mastered the product.

## 2. Where it runs

| Surface | What it's for |
|---------|---------------|
| Terminal CLI (`claude`) | The full-featured daily driver; also scriptable (`claude -p`) |
| Desktop app (Mac/Windows) | Visual diffs, parallel sessions side by side, scheduled tasks, computer use |
| Web (claude.ai/code) | Cloud sessions that keep running after you close the tab; start from any device |
| VS Code / JetBrains extensions | Same engine inside your editor, inline diffs |
| Mobile app | Kick off and steer cloud sessions from your phone |
| GitHub Actions / CI | Claude as automation: review PRs, fix issues, on a schedule or trigger |

Power move most users never find: sessions can move between surfaces. Start in the terminal, send it to the cloud (`/teleport`), check it from your phone at lunch, review the diff on desktop. Long tasks belong in cloud sessions; they survive your laptop lid closing.

## 3. Setup that pays rent every day

### CLAUDE.md: the standing brief

`CLAUDE.md` is loaded at the start of every session in that project. It's your permanent answer to "what would I tell a new engineer on day one?"

- **What goes in:** build/test/run commands, code style choices a linter can't infer, architecture decisions, gotchas, "never touch X".
- **What stays out:** anything Claude can read from the code, standard conventions, essays. Every line costs context on every message; bloated CLAUDE.md files get ignored. Target under ~200 lines.
- **Layers:** `~/.claude/CLAUDE.md` (you, everywhere) → `CLAUDE.md` (project, committed) → `CLAUDE.local.md` (you, this project, gitignored). `/init` drafts one from your codebase.
- **Scale trick:** path-scoped rules in `.claude/rules/*.md` with `paths:` frontmatter load only when Claude touches matching files. Big monorepo knowledge without the standing cost.
- **The habit:** every time you correct Claude and think "it should have known that," add one line. Claude Code also keeps its own auto memory per project (`/memory` to view/edit); review it occasionally like you'd review a new hire's notes.

### Permissions: set your risk dial deliberately

Modes (cycle with Shift+Tab): **default** (asks before acting), **plan** (read-only: explore and propose, never edit), **acceptEdits** (file edits pre-approved), **auto** (a safety classifier reviews routine actions so you don't have to), **bypass/dontAsk** (for sandboxes and CI).

The expert setup isn't a mode, it's an **allowlist**: put your always-safe commands in `.claude/settings.json` once, and stop clicking approve forever:

```json
{
  "permissions": {
    "allow": ["Bash(npm run *)", "Bash(git commit *)", "Read"],
    "deny": ["Read(.env)", "Read(secrets/**)"]
  }
}
```

Settings layer the same way memory does: user → project (committed, becomes team policy) → local overrides. There's even a built-in skill (`/fewer-permission-prompts`) that mines your history and drafts the allowlist for you.

## 4. The extension points

### Skills (Module 01, applied here)

Live in `~/.claude/skills/` or `.claude/skills/`, auto-trigger or run as `/name`. Claude Code is the best place to *develop* skills: `/skill-creator` builds and evals them, and committed project skills ship to your whole team via git.

### Hooks: the guarantee layer

Instructions are requests; hooks are law. A hook is a shell command the harness itself runs at fixed points in the loop; Claude cannot skip it. The events that matter: **SessionStart** (prepare env), **PreToolUse** (validate/block an action before it happens), **PostToolUse** (react after), **Stop** (gate the end of a turn).

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{ "type": "command",
                  "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write" }]
    }]
  }
}
```

That's "every file Claude edits gets formatted, always." Other classics: block edits to protected paths (PreToolUse exit code 2 = deny), run tests on Stop so a turn can't end red, log every command for audit. Rule of thumb: **if it must happen every time, it's a hook, not a sentence in CLAUDE.md.**

### Subagents: more hands, fresh memory

A subagent is a separate Claude with its own context window, prompt, and tool limits. Two ways in: ask ad hoc ("use a subagent to investigate how auth works and report back"), or define reusable ones in `.claude/agents/*.md` (name, description, allowed tools, even a different model). What they solve is **context pollution**: exploring 80 files to answer one question would trash your main session's memory; a subagent absorbs the mess and returns only the conclusion. They also run in parallel and in the background.

### MCP: reach

`claude mcp add` connects external systems: GitHub, Linear, Slack, Figma, Playwright (a real browser), your database. Project-scoped `.mcp.json` is committed so the whole team gets the same connections. One caution the docs underline: every connected server's tools cost context, so connect what you use, prune what you don't.

### Plugins

Bundles of skills + agents + hooks + MCP config, installable in one command from marketplaces (`/plugin`). This is how you package "the way our team works" (or adopt someone else's packaging).

## 5. Daily-driving technique

**Plan first, then build.** For anything non-trivial, start in plan mode (Shift+Tab): Claude explores and proposes; you approve; it executes. Reviewing a plan takes 30 seconds and prevents the expensive failure mode (confidently building the wrong thing).

**Give it a verification loop.** The single biggest predictor of great agentic results: can Claude *check* its own work? Tests, a build command, a linter, a screenshot via Playwright MCP. "Write tests first, then implement until they pass" turns Claude from a code generator into a closed loop that converges. If a task has no natural check, invent one before you start.

**Manage context like a resource, because it is one.** `/context` shows what's loaded. `/clear` between unrelated tasks. `/compact` to summarize a long session and keep going. Long-session drift is real; experts reset early and often, re-briefing with a tight summary instead of dragging 200 messages of history.

**Steer early, rewind fearlessly.** Interrupt the moment direction looks wrong (Esc). Every prompt creates a checkpoint: Esc Esc (or `/rewind`) restores the code, the conversation, or both to any earlier point. This kills the sunk-cost instinct; wrong turns cost seconds, not sessions. (Checkpoints don't cover bash side effects; git covers the rest.)

**Interview trick for big features.** Before implementing something large: "Interview me about this feature; ask about edge cases, tradeoffs, and UX until the spec is unambiguous, then write SPEC.md." Fresh session executes the spec. Claude asks the questions you forgot you needed to answer.

**Adversarial review.** After implementation, spawn a fresh subagent (or second session): "Review this diff against SPEC.md as a skeptical senior engineer." Fresh context has no attachment to the code it's judging; it finds what the author-session can't see.

**Model and effort per task.** `/model` switches models; effort settings trade depth for speed. Crank effort for gnarly debugging and architecture; drop it for mechanical chores; `/fast` (Opus fast mode) when you're iterating live and latency hurts.

## 6. Parallelism: the real productivity unlock

One Claude is an assistant; several are a team. The patterns:

- **Git worktrees** (`git worktree add ../proj-feature`): two checkouts, two terminal sessions, zero interference. Feature in one, bugfix in the other.
- **Cloud fan-out:** fire three claude.ai/code sessions at three independent tasks from the web UI or your phone; review diffs as they land.
- **Writer/reviewer:** session A implements while session B (fresh context) reviews A's diffs.
- **Background tasks:** long builds and dev servers run backgrounded inside a session while Claude keeps working.

Your role shifts from operator to **tech lead of a small team**: you write briefs, review outcomes, and unblock. That role shift *is* the top-1% productivity story.

## 7. Automation: Claude without you present

- **Headless:** `claude -p "task"` runs one-shot, scriptable, with `--output-format json` for pipelines and `--allowedTools` to pre-approve. Composable like any Unix tool: pipe logs in, pipe summaries out, chain instances.
- **CI:** Claude Code in GitHub Actions reviews PRs, triages issues, fixes failing builds on a label. (`/install-github-app` sets this up.)
- **Scheduled:** recurring cloud Routines and desktop scheduled tasks ("every morning, triage new issues and draft fixes for the easy ones").
- **Building on it:** the Agent SDK is this same harness as a Python/TypeScript library, for shipping your own agents (Module 05).

## 8. The habits that compound

1. End sessions by banking learnings: a CLAUDE.md line, a skill, an allowlist entry.
2. Plan mode by default for multi-file work.
3. Never start a task without naming its verification check.
4. `/clear` at every topic change; treat context as sacred.
5. Hooks for musts, instructions for shoulds.
6. Subagents for exploration, main session for decisions.
7. Two parallel sessions minimum once tasks are independent.
8. Let the cloud work while you live; check in from your phone.

## 9. Exercises

1. Run `/init` on a real project, then cut the generated CLAUDE.md to under 60 lines of only what Claude couldn't infer.
2. Add the prettier/lint PostToolUse hook above and watch it fire.
3. Define a `code-reviewer` subagent and run the adversarial-review pattern on a real diff.
4. `claude mcp add playwright`, then ask Claude to open your app, click through a flow, and screenshot each step.
5. Do one full feature with: interview → SPEC.md → plan mode → implement with tests → subagent review.
6. Run two worktree sessions on two real tasks for one afternoon. Notice what your job becomes.

---

*Next: `05-api-and-agent-sdk.md` — building products on the same engine.*

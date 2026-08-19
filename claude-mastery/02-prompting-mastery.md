# Module 02 — Prompting Mastery

Skills (Module 01) encode good prompting so you can stop repeating it. This module is the craft they encode. It transfers to every surface: chat, Projects, Claude Code, the API, other AI products.

---

## 1. The mental model that fixes 80% of bad prompts

**Claude is a brilliant, eager new hire on day one.** Deep general expertise, zero knowledge of your situation, and no ability to read your mind. Every frustration with output quality traces back to briefing this hire badly:

- You know the report is for the board; Claude doesn't unless you say so.
- You know "make it better" means "shorter and less formal"; Claude doesn't.
- You'd never hand a new hire a task with no example of past work. Don't do it to Claude.

Before sending a prompt, ask: *could a talented human do this well with only what I've written?* If not, the model can't either. The gap between average and expert users is mostly briefing quality, not magic words.

## 2. The techniques, in order of leverage

### 2.1 State the outcome and the audience

Weak: "Write something about our Q3 results."
Strong: "Write a 150-word summary of Q3 results for the all-hands slide. Audience: 200 employees, half non-technical. Goal: honest about the revenue miss without tanking morale."

Every prompt that matters gets: what, for whom, what it's for, and how you'll judge it.

### 2.2 Give success criteria, not adjectives

"Make it engaging" is a coin flip. "A busy person should get the point from the first sentence of each paragraph" is checkable. Turn every adjective in your prompt into a test:

| Adjective | Criterion |
|-----------|-----------|
| "concise" | "under 200 words" |
| "professional" | "no exclamation marks, no emoji, contractions fine" |
| "thorough" | "cover cost, risk, and timeline; flag anything you couldn't verify" |
| "clean code" | "passes lint, no function over 40 lines, tests included" |

### 2.3 Show one example (worth 500 words of description)

For anything with a format or a voice, paste a gold-standard example. "Match the tone and structure of the sample below" outperforms any paragraph describing tone. One great example usually suffices; two or three if the pattern has variations. This is the highest ROI-per-second technique on this list.

### 2.4 Separate instructions from data

When a prompt mixes what-to-do with the material to do it on, wrap the material:

```
Summarize the customer complaints in <feedback> for our product team.
Group by theme, most frequent first.

<feedback>
[500 lines of raw survey exports]
</feedback>
```

XML-style tags (`<feedback>`, `<draft>`, `<rules>`) are the convention Claude is trained to respect. For long documents, put the document first and the question after it; if you need grounding, ask Claude to quote the relevant passages before answering. This also inoculates against junk inside the data ("ignore previous instructions" in a pasted email stays data, not instructions).

### 2.5 Give it an out

Add: "If information is missing, ask instead of assuming" or "If you're not sure, say so and list what would settle it." This one line converts confident hallucination into useful questions. For research tasks: "distinguish what you verified from what you inferred."

### 2.6 Set the role when expertise or standards matter

"You are a skeptical security reviewer" produces different attention than no role. Use roles to set *standards* ("senior editor who cuts ruthlessly"), not costumes. Skip it for simple factual asks.

### 2.7 Let it think, and ask for the thinking you need

Current Claude models reason adaptively; hard problems get more thought automatically. You still steer it:

- "List the edge cases before writing the code."
- "Give three candidate approaches with tradeoffs, then recommend one."
- "Argue against your own conclusion before finalizing."

In Claude Code and the API you also have effort controls; crank them for correctness-critical work, drop them for mechanical work.

### 2.8 Build in a verification loop

The expert move: give Claude the means to check its own work, then make checking part of the task.

- Writing: "After drafting, score it against these 4 criteria and revise once."
- Code: "Write the test first, then make it pass" (in Claude Code, this is the whole workflow: Claude runs the tests).
- Analysis: "Recompute the two most important numbers a second way; flag mismatches."

A task with a feedback loop converges; a task without one wanders.

### 2.9 Iterate like an editor, not a slot machine

When output misses, diagnose before re-rolling: name the one thing wrong ("right structure, tone is too salesy; fix only tone"). Targeted correction beats "try again" every time. And in chat surfaces, prefer **editing your original message** over stacking ten corrections; a conversation full of failed attempts pollutes context and drags quality down. Same principle in Claude Code: if a session went sideways, `/clear` or rewind beats arguing with it.

### 2.10 Decompose

One message, one job. "Research the market, write the plan, make the deck, and draft the announcement" invites mediocrity everywhere. Chain it: research → review the research → plan → deck. You are the editor between stages; that's where your judgment enters. (Later modules automate exactly this with subagents and workflows.)

## 3. Prompting agents (Claude Code and friends)

Agentic work adds three rules:

1. **Specify "done."** Agents run until they think they're finished, so define finished: "done = tests pass, lint clean, README updated." Vague goals produce sprawling diffs.
2. **Plan first for anything non-trivial.** "Make a plan before touching files; I'll approve it" (Claude Code has a dedicated plan mode). Reviewing a plan takes 30 seconds; reviewing a wrong 40-file diff takes an hour.
3. **Interrupt early.** The moment direction looks wrong, stop and redirect. Sunk-cost patience with a wandering agent is the most expensive habit in agentic work.

## 4. The 2026 shift the top 1% already made

Prompt guides from 2023-2024 taught heavy scaffolding: rigid step-by-step instructions, "you MUST", chains of hedges. On current models that style often *reduces* quality; Anthropic's own migration docs warn that prompts written for older models are frequently too prescriptive. The modern style:

> **State the goal, the constraints, and the quality bar. Provide the context and examples. Let the model plan the middle.**

Micro-managing the middle wastes your time and fights the model's planning. Keep the scaffolding for what genuinely needs it: compliance rules, exact formats, safety rails. Everything else: brief like you'd brief your best employee, and judge the result against your criteria.

Corollary the top 1% use daily: **ask Claude to write the prompt.** "Here's what I want and what went wrong last time; write the prompt that would have gotten it right the first time." Then save the good ones (Module 01 told you where: skills).

## 5. Anti-patterns (instant tells of an amateur)

- **Adjective soup:** "comprehensive, engaging, professional, impactful" — zero checkable criteria.
- **The kitchen-sink message:** four unrelated asks, one message, all four done poorly.
- **Slot-machine regeneration:** re-rolling without diagnosing the miss.
- **Contradiction stuffing:** "be brief but extremely detailed, casual but formal."
- **Context starvation:** asking for a judgment call while withholding the facts you have.
- **Context hoarding:** pasting 60 pages when the task concerns 2; irrelevant bulk dilutes attention (and money, on the API).
- **Arguing with a derailed session** instead of resetting with a better brief.

## 6. Exercises

1. Take the last prompt that disappointed you. Rewrite with: outcome + audience + 3 checkable criteria + one example. Compare results.
2. Turn three adjectives you habitually use into criteria tables like §2.2.
3. Run one task with a verification loop (§2.8) and one without. Note the difference.
4. Have Claude critique you: paste three of your recent prompts and ask "what does an expert prompter see here? Rewrite each."
5. Take your best rewritten prompt and compile it into a skill (Module 01's loop).

---

*Next: `03-claude-ai-power-use.md` — the daily-driver surface.*

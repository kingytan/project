# Module 01 — Skills: Installable Expertise

> **The one-sentence version:** A skill is a folder of instructions, reference files, and scripts that Claude loads on demand when a task calls for it. You write the expertise once; Claude applies it every time after, on every Claude surface you use.

This is the first module because skills are the highest-leverage concept in the whole Claude ecosystem. Everything else you will learn (prompting, Claude Code, the API) gets more valuable once you think in skills.

---

## 1. What a skill actually is

Mechanically, a skill is a folder:

```
my-skill/
├── SKILL.md          # required: instructions + YAML frontmatter
├── references/       # optional: deep material, loaded only when needed
│   ├── style-guide.md
│   └── examples.md
├── scripts/          # optional: executable code Claude runs
│   └── validate.py
└── templates/        # optional: files Claude copies or fills in
    └── report.md
```

`SKILL.md` starts with YAML frontmatter, then Markdown instructions:

```markdown
---
name: quarterly-report
description: Generate the quarterly business review deck in house format.
  Use when the user asks for a QBR, quarterly report, board update, or
  business review. Pulls numbers from metrics.csv, never invents figures.
---

# Quarterly Report

## Steps
1. Read metrics.csv and compute quarter-over-quarter deltas.
2. Run scripts/validate.py to check the numbers reconcile.
3. Fill templates/report.md. Follow references/style-guide.md for voice.
...
```

That is the whole format. No SDK, no build step, no registration. Files in a folder.

### The mechanism that makes skills special: progressive disclosure

This is the core design insight, and most users never learn it:

| Level | What loads | When | Cost |
|-------|-----------|------|------|
| 1 | `name` + `description` only | Session start, for every installed skill | A few dozen tokens each |
| 2 | The full `SKILL.md` body | Only when your task matches the description | One file's worth |
| 3 | `references/`, `templates/` | Only the specific file the task needs | Pay per use |
| — | `scripts/` | Executed, output returned; the code itself never occupies context | Near zero |

Claude's context window is its working memory, and it is finite. Progressive disclosure means you can install fifty skills and the idle cost is a couple of paragraphs. A skill can contain a 200-page procedure manual in `references/` and Claude only reads the chapter the current task needs. **Skills give Claude effectively unbounded expertise at near-zero standing cost.** No other customization mechanism has this property: everything you put in project instructions or CLAUDE.md is paid for on every single message, relevant or not.

### Where skills run

The same folder format works across every Claude surface:

| Surface | Where they live |
|---------|----------------|
| Claude Code (CLI, desktop, web) | `~/.claude/skills/` (yours, all projects), `.claude/skills/` (this repo, shared via git), or bundled in plugins |
| claude.ai web + desktop + mobile apps | Enabled per-account in Settings → Capabilities; upload your own, or use Anthropic's built-ins (Excel, Word, PowerPoint, PDF creation) |
| Claude API | Skills API (`/v1/skills`) + the code-execution container: `container={"skills": [...]}` |
| Claude Agent SDK / Managed Agents | Same folder format, loaded into your custom agents |

Write once, use everywhere: the skill you build for Claude Code works when you upload it to claude.ai, and when your company later automates the workflow through the API. This portability is why skills beat "a prompt I keep in my notes app."

### How a skill gets invoked

Two paths:

1. **Automatic.** Claude reads every installed skill's `description` at session start. When your request matches one, Claude loads and follows it. You say "clean up this draft" and a well-described editing skill just activates.
2. **Explicit.** In Claude Code, every skill is also a slash command: `/stop-slop`, `/quarterly-report q3 numbers attached`. Explicit invocation is a guarantee; automatic is a convenience.

Frontmatter gives you control knobs (Claude Code): `disable-model-invocation: true` makes a skill manual-only (useful for destructive workflows like deploys), and `allowed-tools` restricts what tools it may use while active.

---

## 2. When to use a skill (and when to use something else)

### The signals that say "make this a skill"

1. **You have typed the same instructions twice.** The third repetition is you doing a skill's job by hand. This is the single most reliable signal.
2. **There is a right way to do it.** A procedure with steps, an order, and checks. ("First reconcile the totals, then draft, then verify no client names appear.")
3. **Output must match a format or voice.** Brand style, report template, code convention, your personal writing taste.
4. **Claude needs knowledge it cannot already have.** Your internal API, your team's review rubric, your client's quirks, your family's dietary constraints.
5. **Part of the job is deterministic.** Anything a script can do (parse the export, validate the totals, resize the images) belongs in `scripts/`, not in the model's improvisation.

### When it is NOT a skill

| You want | Use instead | Why |
|----------|-------------|-----|
| A rule that applies to every message in a project ("we use pnpm, never npm") | `CLAUDE.md` / project instructions | Always-on context belongs in always-loaded files; skills load on demand |
| A one-off task | Just prompt | Skills are for repetition |
| Access to data or systems (Gmail, Jira, your database) | Connectors / MCP servers | MCP gives Claude *reach*; skills give Claude *technique*. They compose: a skill can direct how to use MCP tools |
| Facts about you remembered across chats ("I prefer short answers") | Memory | Memory is passive recall; skills are active procedure |
| A big isolated investigation (audit 300 files) | Subagents | Subagents give fresh context windows; a skill can *instruct* subagent use |

Keep the two axes straight and you will out-architect most users: **CLAUDE.md = always loaded, keep tiny. Skills = loaded on demand, can be huge. MCP = access. Skills = know-how.**

---

## 3. Dissect the skill you already own

Your repo already ships one: `.claude/skills/stop-slop/`. Open it and study the architecture, because it is textbook:

```
stop-slop/
├── SKILL.md                    # 8 core rules + a quick checklist (~90 lines)
└── references/
    ├── phrases.md              # long ban-list, loaded only when editing prose
    ├── structures.md           # formulaic patterns to break
    └── examples.md             # before/after transformations
```

Notice three deliberate choices:

1. **The description states its trigger**: "Use when drafting, editing, or reviewing text..." — that sentence is what makes it fire automatically when you ask Claude to "polish this email."
2. **SKILL.md is a router, not an encyclopedia.** The 8 rules fit in one screen. The heavy material (hundreds of banned phrases, worked examples) lives in `references/` and loads only when needed.
3. **It includes a verification step** (the scoring rubric). Good skills don't just say how to do the work; they say how to check it.

Every skill you write should copy this shape.

---

## 4. Real scenarios for you

### Work scenario: the Monday status update

You (like nearly everyone) produce some recurring artifact: a weekly status update, a client summary, a standup report. Every week you re-explain the format, the tone, what to include, what your manager cares about. That re-explanation *is* a skill, still in interpreted mode. Compile it:

```markdown
---
name: weekly-status
description: Write my Monday status update for my manager. Use when I ask
  for my weekly update, status report, or Monday summary. Input is my raw
  notes or the week's git log / calendar; output follows the exact format
  in templates/status.md.
---

# Weekly Status Update

## Rules
- Three sections, always: Shipped / In progress / Blocked+Risks.
- Lead every bullet with the outcome, not the activity
  ("Cut page load 40%" not "Worked on performance").
- Blocked items name the person or decision needed. Never vague.
- Under 200 words. My manager reads on a phone.
- Tone: plain, confident, zero filler. No "just", no "hopefully".

## Steps
1. Ask for raw inputs if none given (notes, git log, calendar).
2. Draft using templates/status.md.
3. Check every bullet against references/good-vs-bad-bullets.md.
4. Flag anything that sounds like activity instead of outcome.
```

Monday morning becomes: paste your raw notes, type "weekly update", get a finished draft in your exact format. Ten minutes to write the skill; saved every week, forever, and the quality is *more* consistent than when you prompted from scratch.

### Personal scenario: the trip planner

Personal life is where skills are most underused. Anything you do repeatedly with rules attached qualifies. Example:

```markdown
---
name: trip-planner
description: Plan family trips my way. Use when I ask to plan a trip,
  vacation, itinerary, or getaway.
---

# Trip Planning

## Constraints (non-negotiable)
- Two adults, one 6-year-old: max one "big activity" per day, hotel
  pool required, first dinner near the hotel.
- Budget ceilings: hotels $250/night, prefer breakfast included.
- I hate: hop-on-hop-off buses, restaurants with QR-code-only menus.
- Always produce: day-by-day table, booking checklist with deadlines,
  packing list keyed to weather.

## Steps
1. Confirm dates, destination, and any one-off constraints.
2. Research current options (flag anything that needs a live price check).
3. Output the three artifacts above. Itinerary includes backup
   rainy-day option per day.
```

Now "plan us 4 days in Osaka in November" produces something tailored to your family on the first try, in claude.ai on your phone, because the skill carries the twenty preferences you'd never bother typing each time. Same pattern works for: meal planning around your diet, a monthly budget review that reads your bank CSV export (pair it with the built-in Excel skill), gift research, a workout programmer, a kid's-homework-helper with your rules about not giving answers away.

---

## 5. What the top 1% know and do with skills

### Secret 1: The description is the product

Most skills fail at *retrieval*, not content: Claude never loads them because the description doesn't match how the user actually phrases requests. At session start, the description is the **only** thing Claude sees. Amateurs write labels ("Helps with reports"). Experts write trigger specifications:

```yaml
# Amateur — never fires
description: A skill for better writing.

# Expert — fires exactly when it should
description: Remove AI writing patterns from prose. Use when drafting,
  editing, reviewing, or polishing any text - emails, docs, posts -
  or when the user says it "sounds like AI", "too wordy", "make it
  human". Do not use for code comments.
```

The formula: **what it does + when to use it + the literal words a user would say + when NOT to use it.** After writing one, test it: phrase the task three different ways in fresh sessions and check the skill loads each time. If it doesn't, tune the description, not the body.

### Secret 2: Ship scripts, not prose

The single biggest quality gap between average and expert skill authors: experts push every deterministic step into a bundled script and reserve the model for judgment. Anthropic's own document skills work this way; the Excel skill doesn't *describe* how to build a spreadsheet, it ships Python that does it flawlessly, and Claude runs it.

Ask of every step: "does this have exactly one correct output?" Parsing a CSV, validating totals, renaming files, checking a date format: script. Deciding what the numbers *mean*: model. Scripts are faster, cost almost no tokens, and never hallucinate.

### Secret 3: The compile loop (this is the big one)

Top-1% users treat prompts as source code and skills as the compiled binary. Their habit: **the moment a session produces a great result, they end it with "turn what we just did into a skill."**

Claude Code ships a `skill-creator` skill for exactly this: it interviews you, drafts the folder structure, writes the trigger description, and can even run evals against it. The workflow:

1. Do a task with Claude, iterating until the output is exactly right.
2. Say: "Capture this as a skill. Everything we corrected along the way becomes a rule."
3. Next time, the corrections are pre-applied.

Every mistake you correct once becomes a rule Claude never breaks again. Average users have the same conversation with Claude every week and lose the learning each time; the session ends and the coaching evaporates. Top users convert coaching into infrastructure. Their Claude gets measurably better every single week, and the gap compounds.

### Secret 4: Test skills like software

Experts treat a skill as code: version-controlled (yours already is: it lives in a git repo), changelogged (stop-slop has a CHANGELOG.md), and *tested*. `skill-creator` can benchmark a skill: run realistic tasks with and without it, compare outputs, measure whether the description triggers reliably. When a skill misbehaves, they don't rewrite from scratch; they add the failing case to the skill the way you'd add a regression test.

### Secret 5: Small skills compose

One giant "do-everything-for-marketing" skill performs worse than five small ones (brand-voice, campaign-brief, competitor-teardown, launch-checklist, post-mortem) because Claude loads exactly what the task needs and combines them: one request can fire brand-voice + the PowerPoint skill + a data-analysis skill together. Build a library of sharp single-purpose tools, not a Swiss Army knife with fifty blades open at once.

### Secret 6: Skills are how teams scale taste

On Team/Enterprise plans, admins can distribute skills org-wide; in Claude Code, a skill committed to `.claude/skills/` ships to every teammate on `git pull`, and plugins/marketplaces distribute them publicly. The implication: **the best practitioner's judgment becomes everyone's default.** Your best reviewer's checklist, your best writer's voice, encoded once, applied by everyone. This is the difference between "we have a style guide nobody reads" and "the style guide enforces itself."

---

## 6. How this changes the way you work

Without skills, output quality depends on how well you prompt *today*, and your best prompting evaporates when the chat ends. With a skill library:

- **Prompting shrinks to intent.** "Do the weekly report" replaces three paragraphs of instructions. The three paragraphs are installed.
- **Quality becomes a floor, not a coin flip.** The skill applies rule #7 every time, including the ones you'd forget.
- **Delegation becomes trustworthy.** You review outcomes instead of supervising mechanics, which is the actual unlock for using Claude daily at work.
- **Your setup compounds.** Week 10 you is served by nine weeks of accumulated, tested expertise. This is the moat between the top 1% and everyone else: they are not better prompters, they have stopped re-prompting.

---

## 7. Exercises (do these this week)

1. **Dissect the specimen.** Read `.claude/skills/stop-slop/SKILL.md`, then its `references/`. Note the router-plus-references shape.
2. **Mine your history.** List the last 3 instructions you've given Claude (or any AI) more than twice. Those are your first three skills. If stuck, check: recurring documents, recurring decisions, recurring formats.
3. **Build one.** In Claude Code run `/skill-creator` and build the work scenario above, adapted to your real weekly artifact. Keep SKILL.md under 100 lines.
4. **Test the trigger.** Fresh session, phrase the task three ways. Tune the description until 3/3 fire.
5. **Add one script.** Find one deterministic step and move it to `scripts/`.
6. **Install the habit.** For one week, end every good Claude session with: "Should any of this become a skill? If yes, draft it." That habit alone puts you in the top 1%.

---

*Next module: `02-prompting-mastery.md` — the craft that skills encode.*

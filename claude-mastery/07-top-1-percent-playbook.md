# Module 07 — The Top 1% Playbook

Everything before this was knowledge. This module is behavior: what the best users *do differently*, distilled to principles you can audit yourself against. None of it requires talent; all of it requires habit.

---

## 1. They build systems, not conversations

The defining difference. Average users have good sessions; top users **capture** good sessions:

- A great result → a skill (Module 01's compile loop).
- A repeated correction → a CLAUDE.md line or project instruction.
- A repeated approval → a permission allowlist entry.
- A repeated sequence → a hook, a Routine, or a saved workflow.

Ask of every friction: "am I solving this, or solving it *forever*?" One is work, the other is investment. After three months, the investor's Claude is unrecognizably better than the visitor's, on identical subscriptions.

## 2. They delegate outcomes, not steps

Novices use Claude like autocomplete: "write a function that..." fifty times a day. Experts hand over whole outcomes with clear success criteria: "Ship the CSV export feature. Done = tests pass, handles the 1M-row case, docs updated." Then they review the result like a lead reviews a PR.

The trust ladder, climbed deliberately:
1. Claude drafts, you rewrite → 2. Claude produces, you edit → 3. Claude produces and self-checks, you review → 4. Claude runs unattended, you audit outcomes.

Each rung needs the previous one's track record plus a verification mechanism. Most users park at rung 1 out of habit, not evidence. Top users push each task type up the ladder as fast as its error cost allows, and their day fills with rung-4 work running in the background.

## 3. They are ruthless about context

Context is Claude's working memory, and they treat it like a scarce resource:

- Curate what goes in: the relevant 2 pages, not the whole 60-page PDF.
- Reset often: new topic, new chat (`/clear`); a long, messy thread degrades quality invisibly.
- Isolate mess: exploration goes to subagents; the main thread holds decisions.
- Persist what matters *outside* the window: skills, CLAUDE.md, project knowledge, memory. The window is for the task; the system is for the knowledge.

## 4. They give every important task a feedback loop

The universal quality trick, in any surface: **make Claude check its own work against something.** Tests. A rubric. A checklist. A second pass in a fresh context. "Score your draft against these five criteria, then revise" is 15 words and routinely doubles quality. A task with a loop converges; a task without one is a coin flip you keep re-flipping.

Corollary they exploit everywhere: **generating and judging are different jobs.** The same model that wrote something finds more flaws when asked to judge it in a fresh context with a critic's brief. Draft with one session, audit with another.

## 5. They pick the right surface without thinking

| Job | Surface |
|-----|---------|
| Think, draft, decide, learn | claude.ai chat (with Projects for anything recurring) |
| Recurring workflow with context | A Project + skills |
| Anything touching files, repos, data, many steps | Claude Code |
| Long tasks that shouldn't need you present | Cloud sessions, Routines, scheduled tasks |
| While browsing / inside other tools | Chrome extension, Slack, Excel integrations |
| A product or pipeline for others | API / Agent SDK |

The tell of an amateur: doing a 40-file refactor by pasting code into chat, or asking the API-shaped question in a surface with no tools. Match the job to the surface and half your "Claude is bad at this" moments disappear.

## 6. They run a portfolio, not a queue

Top users are rarely waiting on Claude. While the big refactor runs in a cloud session, they're in chat shaping tomorrow's plan, with a research Routine filling a doc in the background. Three streams: **interactive** (you, live), **delegated** (running now, review later), **scheduled** (recurring, audit weekly). If you are ever watching a progress spinner, you have spare capacity to deploy.

## 7. They know what not to delegate

Judgment about what matters, taste, relationships, accountability, and anything they'd be unable to verify. The top 1% are *more* careful here than novices: they delegate aggressively where verification exists and conservatively where it doesn't (irreversible actions, high-stakes external communication, decisions that are theirs to own). Claude amplifies your direction; being wrong faster is not a feature. They also stay the author of their own thinking: Claude sharpens their draft logic instead of replacing it wholesale, because a tool you can't evaluate is a tool you can't trust.

## 8. They upgrade the meta-loop

Once a month, they ask Claude itself: "Here's how I've been using you (paste examples). What would a top 1% user do differently? What should become a skill, a Project, a Routine?" The tool critiques your use of the tool. Almost nobody does this; it is free coaching, forever.

And they keep current on ~15 minutes a month (news page, changelogs, release notes), because each capability jump reshuffles what's delegable. The people who noticed agents early got a year of compounding before the average user caught up.

---

## The self-audit

Score yourself 0–2 on each ("no / sometimes / habitually"):

1. Good sessions end with something captured (skill, instruction, allowlist line).
2. Important tasks are briefed with outcome + checkable criteria.
3. Every important task has a verification loop.
4. Context stays curated; new topic = new chat; big docs get trimmed.
5. Exploration runs in subagents/background, not your main thread.
6. At least one recurring workflow runs on a schedule without you.
7. You review outcomes instead of watching Claude work.
8. You've asked Claude to critique your usage in the last 30 days.
9. You know exactly which surface each job goes to.
10. Something you built (skill/Project/agent) served you today without new prompting.

**16+ = top 1% behavior. Under 10 = you're leaving most of the product on the table.** Retake monthly; the score moves fast once the capture habit lands.

---

*Next: `08-thirty-day-path.md` — the habits above, installed one day at a time.*

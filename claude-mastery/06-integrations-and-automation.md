# Module 06 — Claude Everywhere: Cowork, Integrations, Automation

Modules 03–05 covered the surfaces you go *to*. This module covers Claude coming to *you*: the agent that does knowledge work while you're away, and Claude living inside the browser, Slack, and Office where your work already happens. For "use Claude daily in work and personal life," this is the module that changes your calendar.

---

## 1. Cowork: agentic work for people who don't write code

Claude Code proved that an agent with tools and a loop beats a chat box for real work. **Cowork is that model pointed at knowledge work.** You describe an outcome; Claude plans, works across many steps, and hands back finished deliverables: organized files, drafted documents, synthesized research, populated spreadsheets. You review the approach, step away, come back to results.

Where it runs: the desktop app (Mac/Windows, Linux beta) on any paid plan, with web and mobile in beta. It shares the same home as Chat on claude.ai, chosen from the message box.

**What makes it different from chat:**
- It **works on your actual files and apps**, not on pasted text.
- It runs **multi-step and unattended** — you're not babysitting each turn.
- You can **jump in mid-task** to redirect, then leave again.
- It **schedules** (`/schedule` in any task, or the Scheduled sidebar), so recurring work runs without you.
- On desktop it can use **computer use**: clicking, typing, and navigating your actual applications when no connector or browser path exists.

**Safety posture worth understanding before you enable computer use:** connectors are tried first, then Claude in Chrome, then computer use as the last resort. It asks permission per app, blocks finance and crypto apps by default, and there's **no sandbox between Claude and your applications** — it acts as you, on your machine, which must stay awake with the app open. Treat it like handing someone your unlocked laptop: fine for the tasks you'd delegate to a capable assistant, not for anything you couldn't verify or undo.

**Real Cowork jobs (work):** turn a folder of raw interview notes into a structured findings doc; reconcile three spreadsheets into one report; prepare a client folder for a meeting (pull the docs, summarize the history, draft the agenda); weekly competitor sweep saved to a running doc.

**Real Cowork jobs (personal):** organize the photo/document dump into a sane folder tree; assemble the tax-time file; research a big purchase across a dozen sources and produce a decision matrix; keep a household inventory or reading list updated on a schedule.

The mindset shift: stop asking "how do I get Claude to help me with this task?" and start asking **"what's the outcome, and what does done look like?"** — then let it work. Cowork rewards outcome briefs and clear definitions of done exactly like Claude Code does (Module 02, §3).

## 2. Claude in Chrome

An extension with a side panel that stays open while you browse. Claude reads the page you're on and can act: click, type, navigate, fill forms. Since August 2026 that side panel *is* a Cowork session, so browser work is saved to your history and can continue elsewhere.

Use it for: comparison shopping across tabs, filling repetitive web forms, extracting structured data from a page that has no export, "read this doc and tell me what changed since last quarter," booking and account admin that lives in a clunky web UI.

Caution that matters: a page can contain text designed to hijack an agent reading it (prompt injection). Anthropic ships classifiers and permission prompts, but the discipline is yours — keep it to sites you trust, watch what it does on anything transactional, and never let it act on credentials or payments unattended.

## 3. Claude Tag (Slack)

Claude in Slack became **Claude Tag** in August 2026 (Team/Enterprise beta). You @-mention it in a channel or thread and it works with its own identity: it reads the channel's context, uses your org's connected tools, and follows up on its own. It can spin up Claude Code sessions from a conversation — a bug report in a thread becomes a pull request.

The pattern to steal: **Claude as a team member in the room** rather than a tool each person opens privately. Triage in-channel, summarize long threads for someone catching up, turn a decision into a ticket, answer questions from your connected docs. Same injection caution: only in channels you trust, since anyone who can post can address it.

## 4. Claude for Microsoft 365

Claude inside **Excel, PowerPoint, and Word** (generally available on paid plans) and **Outlook** (beta), installed from Microsoft AppSource. The compounding feature is **cross-app coordination**: read the model in Excel, build the deck in PowerPoint, draft the email in Outlook — one request spanning three apps.

Excel is the standout for most people: Claude works with the real workbook, understands formulas and structure, and edits in place rather than handing you a rewritten file to paste. If your work lives in spreadsheets, this is the single highest-value integration on the list.

## 5. Claude Design

Chat-to-canvas visual design: prototypes, slides, one-pagers, landing pages, posters. You describe it, Claude renders it on a canvas, then you refine by chatting, commenting inline, or editing directly (drag, resize, align, edit text). It supports design systems so output stays on brand. Beta on paid plans at claude.ai/design.

The unlock for non-designers: you can now produce something visually credible without a designer or a template library, and iterate on it in words. For designers: it's a fast first-draft machine that keeps your system's rules.

## 6. Automation: work that happens without you

Four mechanisms, roughly in order of setup cost:

1. **Scheduled Cowork tasks** — `/schedule` inside any task. Best for recurring knowledge work: Monday briefings, weekly digests, monthly reports.
2. **Claude Code Routines and cloud sessions** — scheduled or triggered agent runs in the cloud; monitor from your phone. Best for anything touching repos, files, or data.
3. **GitHub / CI integration** — Claude reviews PRs, fixes CI failures, triages issues on events. Best for engineering hygiene that should never wait on a human.
4. **The API on your own schedule** — cron plus a script, or Managed Agents with built-in scheduling. Best when it's a product or a pipeline (Module 05).

**How to choose what to automate.** A task qualifies when it is (a) recurring, (b) has a stable definition of done, and (c) is verifiable after the fact. That third one is the gate people skip. Automate the weekly competitive digest (easy to check, low cost if imperfect); don't automate sending client emails until you've watched the drafts for a month.

**The starter three**, in the order that pays off fastest:
- A **morning brief**: calendar + inbox + your priorities, delivered before you start.
- A **weekly digest** of something you'd otherwise skim badly (industry news, repo activity, support tickets).
- A **recurring chore** you resent: expense categorization, file cleanup, status assembly, reading-list triage.

Set them up, then audit the outputs weekly for a month. Automation you don't audit turns into noise you ignore.

## 7. Connecting your own systems

Everything above rides on connectors and MCP servers. Three rules the top 1% follow:

- **Connect the sources you actually reason over** (email, calendar, docs, tickets), not everything available. Each connection is context cost and attack surface.
- **Start read-only, earn write.** Let it draft; you send. Then let it send the low-stakes category. Then widen.
- **Know where the boundary is.** A connected agent acting on external content (web pages, emails, Slack messages, PRs) can be targeted by that content. Anything irreversible or outward-facing stays a human decision until you've built real evidence.

## 8. Exercises

1. Run one full Cowork task end-to-end: give it an outcome that takes you 45 minutes manually, walk away, and evaluate the deliverable.
2. Schedule one recurring task (morning brief or weekly digest). Audit it every day for a week; tune the brief.
3. Install Claude in Chrome. Use it for one task you'd normally do across five tabs.
4. If your work touches spreadsheets or decks: install Claude for M365 and do one real deliverable in place.
5. Pick one chore you resent. Decide honestly whether it passes the recurring/done/verifiable test — then automate it or write down why it fails.

---

*Next: `07-top-1-percent-playbook.md` — the habits that tie all of this together.*

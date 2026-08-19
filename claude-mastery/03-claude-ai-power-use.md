# Module 03 — claude.ai Power Use

claude.ai (web, desktop, iOS/Android) is the daily-driver surface: thinking, writing, research, documents, and life admin. Most people use 20% of it: they open a fresh chat, type a question, copy the answer out. This module is the other 80%.

(Its agentic sibling, **Cowork**, shares the same home and handles multi-step work while you're away; it gets its own treatment in Module 06.)

---

## 1. Projects: stop starting from zero

A Project is a workspace with three assets: **custom instructions** (a standing brief applied to every chat inside), **knowledge** (files Claude can always see), and the chats themselves. It's the chat-surface equivalent of Module 01's skills: context you install once instead of retyping.

Create one per *recurring context*, not per topic: "Acme client," "Job search," "Kitchen renovation," "Team newsletter." The test: if you'd brief a human assistant before each task of this kind, that brief is the project instructions.

Available on every plan (Free is capped at five projects). Two mechanics worth knowing: project knowledge is **cached**, so re-using the same reference docs across many chats barely dents your usage limits; and when a project's knowledge outgrows the context window, it switches to retrieval mode automatically, expanding effective capacity roughly tenfold on paid plans.

**Power habits:**
- Write instructions like a skill: who you are, what good output looks like, standing constraints, format rules. Concrete beats aspirational.
- Curate knowledge; don't dump. Ten well-chosen documents beat a hundred stale ones — retrieval quality follows curation quality. Refresh files when reality changes.
- End sessions with "anything worth adding to this project's instructions?" The project gets smarter weekly (the compile loop again).
- Projects can be shared with teammates on team plans: shared context = consistent output across the whole team.

## 2. Memory and hygiene

Claude remembers things about you across conversations: preferences, ongoing projects, context. It's on every plan now, including Free. Treat memory as a system you *manage*, not magic:

- **Audit it.** Settings → Capabilities shows your memory exactly as Claude sees it; edit inline, delete entries, or just tell Claude in chat what to change. **Pause** keeps it but stops using it; **reset** deletes permanently.
- **Scope it.** Each Project keeps its own memory, which is the clean way to separate work from personal life.
- **Move it.** Memory can be exported and imported, including from other AI providers — your accumulated context isn't locked in.
- **Go off the record** when you want no trace: incognito chats aren't saved to history or memory, aren't searchable, and are never used for model improvement.
- Memory is for durable facts ("prefers concise answers," "runs a bakery"); Projects are for working context; skills are for procedures. Right knowledge, right layer.

## 3. Connectors: Claude with your data

Connectors (built on MCP, the same protocol from Module 04) plug Claude into Google Drive, Gmail, Calendar, and a directory of tools like Linear, Notion, Slack, and Figma. This flips the workflow: instead of you fetching context *for* Claude, Claude fetches context for itself.

The unlock is **cross-tool questions**: "Look at tomorrow's meetings, pull the related docs from Drive, and brief me" or "Find the thread where we discussed pricing and summarize what we committed to." Once two or more sources are connected, Claude does the joining that used to be your morning.

Trust ramp: start with read-heavy asks, review outputs, then let it draft (emails, events, tickets) with you approving sends. Same trust ladder as Module 07.

## 4. Research mode: delegate the deep dive

For questions that deserve hours, not seconds: Research mode (paid plans) runs multi-step investigations across the web and your connected sources, then returns a synthesized, **cited** report. Brief it like you'd brief an analyst:

> "Compare the top 5 project-management tools for a 12-person agency. Criteria: price at 12 seats, client-access features, integration with Google Workspace. I need a recommendation with tradeoffs, not a survey. Flag anything you couldn't verify."

Decision framing ("recommend X under constraints Y") beats topic framing ("tell me about project management tools") every time. Then spot-check the citations that carry the conclusion — expert users verify the two load-bearing facts and trust the scaffolding.

## 5. Artifacts: outputs you can use, not just read

Artifacts are standalone creations rendered next to the chat: documents, diagrams, web pages, and fully interactive apps (Claude writes real code). They're versioned as you iterate, shareable by link, and — the part most people miss — **can themselves use Claude**: you can ask for tools like "a flashcard app that generates new cards about any topic I type," then share that app with anyone.

The economics of that are a genuine secret: when someone else uses your AI-powered artifact, **the AI usage counts against their Claude account, not yours**. You can build and share a working internal tool with no hosting, no API key, and no bill.

Power pattern: stop asking for *information* when you actually want an *instrument*. Not "explain mortgage tradeoffs" but "build me a calculator comparing these three loans as I adjust the down payment." Not "quiz me on Spanish" but "build a quiz app from my vocab list."

## 6. Real files in, real files out

Claude reads what you throw at it: PDFs, spreadsheets, images, screenshots, whole datasets. And with the built-in document skills it *produces* real files: formatted Excel models with working formulas, Word docs, PowerPoint decks, PDFs. That closes the loop on office work: raw CSV in → analyzed, charted, formatted deliverable out.

Screenshots are the most underused input: photograph the whiteboard, screenshot the error, snap the wine list. Vision plus context ("we're celebrating, under $60") turns the camera into a query language.

## 7. Voice and mobile

The mobile app is not a lesser Claude; it's the ambient one: voice conversations while commuting (think out loud, get pushback, arrive with the decision made), camera as input, and dictated brain-dumps that end with "turn that into a plan." Friction you remove here shows up as usage: the top 1% talk to Claude at moments desktop users lose entirely.

## 8. Skills on claude.ai

Module 01's skills aren't a Claude Code feature; they work here too, on every plan including Free. Manage them under Customize → Skills: build one by talking to Claude, or upload a folder as a ZIP. They apply across chats, Projects, Cowork, and Code, and organization admins can provision them company-wide. Anthropic's built-in document skills are why claude.ai can hand you a real .xlsx with working formulas.

The same directory also holds your Connectors and Plugins, which is the fastest way to see everything customizing your Claude in one place. Audit it monthly.

## 9. Claude Code from the app

Coding-agent sessions (Module 04) can run in the cloud from the same account: kick off a task at claude.ai/code, close the laptop, review the result from your phone. Even for non-engineers this matters: "agent + repo + schedule" is how recurring digital chores get automated, and the app is the remote control.

## 10. A daily-driver blueprint

- **Morning:** one connected-Claude briefing ("calendar + inbox: what actually needs me today?").
- **During work:** each recurring workstream lives in its Project; drafts, analysis, and decisions happen there, not in fresh chats.
- **Deep questions:** dispatched to Research mode while you keep working.
- **Deliverables:** produced as artifacts/files, not prose you reformat by hand.
- **Evening/personal:** the household runs through Claude too — meal plans against what's in the fridge (photo), trip plans via your trip skill, big-purchase research with a decision matrix.

Every element above is the same five moves: install context once (Projects/skills), connect data (connectors), delegate depth (Research), demand usable outputs (artifacts/files), and capture what worked.

## 11. Exercises

1. Create two Projects today: your biggest work context and one personal ("health," "house," "kids' school"). Write real instructions for each.
2. Memory audit: ask Claude what it remembers about you; correct three things.
3. Connect one data source and run a cross-tool question you'd normally assemble by hand.
4. Run one Research-mode investigation framed as a decision. Spot-check two citations.
5. Replace one "explain X" habit with "build me an instrument for X" (artifact).
6. Upload a messy real spreadsheet; ask for the analysis *and* a formatted Excel deliverable back.

---

*Next: `04-claude-code-mastery.md` — the agent surface.*

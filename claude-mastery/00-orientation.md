# Module 00 — Orientation: The Whole Landscape

Before technique, the map. Most people know one Claude (a chat box). There are three distinct things wearing the name, and knowing which is which is the first thing that separates you from the average user.

1. **The models** — the intelligence itself (Fable 5, Opus 5, Sonnet 5, Haiku 4.5).
2. **The products** — the surfaces you use it through (claude.ai, Cowork, Claude Code, Design, Chrome, Slack, Excel).
3. **The platform** — the API and SDKs you build on.

---

## 1. The models (August 2026)

| Model | Context | Price per MTok (in/out) | What it's for |
|-------|---------|------------------------|---------------|
| **Claude Fable 5** | 1M | $10 / $50 | The frontier: hardest reasoning, long-horizon autonomous agents. First publicly available model of the new Mythos-class tier that sits above Opus. Thinking is always on. |
| **Claude Opus 5** | 1M | $5 / $25 | Near-Fable capability at half the price. Complex agentic coding, enterprise work, deep analysis. The default heavyweight. |
| **Claude Sonnet 5** | 1M | $2 / $10 | Best speed-to-intelligence ratio. The workhorse; the default on Free and Pro. |
| **Claude Haiku 4.5** | 200K | $1 / $5 | Fast and cheap, near-frontier quality. Real-time apps, high volume, subagents. |

Also live: Opus 4.8/4.7/4.6 and Sonnet 4.6 (previous generations), plus **Claude Mythos 5** — the same engine as Fable 5 without its safety classifiers, restricted to approved organizations doing defensive cybersecurity work under Project Glasswing. That pairing is the deliberate design: one model tier, released twice, with the dual-use guardrails as the difference.

**Three things about current models that change how you work:**

- **Adaptive thinking.** Claude decides how much to reason based on difficulty; you no longer manage a thinking budget. You steer it with an **effort** setting (low → max). On Fable 5, thinking is permanently on.
- **1M-token context.** An entire codebase, a year of documents, a full book in a single conversation. The old habit of aggressive summarizing before asking is mostly obsolete; curation still matters, truncation doesn't.
- **Every model is multimodal.** Screenshots, photos, PDFs, charts. The camera is an input device.

**The laddering habit:** experts don't pick one model, they match model to task tier. Haiku triages, Sonnet does the bulk, Opus/Fable take the genuinely hard 5%. Same for effort: low for mechanical work, xhigh/max when correctness is the product.

## 2. The products

| Surface | What it is | Use it for |
|---------|-----------|-----------|
| **claude.ai (web/desktop/mobile)** | Chat, Projects, Artifacts, Research, file creation, voice | Thinking, writing, analysis, deciding — the daily driver (Module 03) |
| **Claude Cowork** | Agentic knowledge work: describe an outcome, walk away, come back to finished deliverables | Multi-step non-coding work: research, document production, file wrangling, ops (Module 06) |
| **Claude Code** | The coding/agent harness (CLI, desktop, web, IDE, CI) | Anything touching files, repos, data pipelines, automation (Module 04) |
| **Claude Design** | Chat-to-canvas visual design with a real editor | Mockups, slides, one-pagers, landing pages, posters |
| **Claude in Chrome** | Side-panel agent that reads and acts on web pages | Browsing-context work, form filling, in-page research |
| **Claude Tag (Slack)** | Claude as a teammate you @-mention in channels | Team workflows, triage, turning threads into work |
| **Claude for Microsoft 365** | Claude inside Excel, PowerPoint, Word, Outlook | Spreadsheet modeling, decks, docs, email — in place |
| **API + Agent SDK + Managed Agents** | The platform | Products, pipelines, custom agents (Module 05) |

The competence that matters here is **surface selection**. The same request lands very differently depending on where you make it: "reorganize these 400 files" is painful in chat, trivial in Cowork or Claude Code. Module 07 has the routing table; internalize it early.

## 3. The plans

| Plan | Price | What you get |
|------|-------|--------------|
| **Free** | $0 | Sonnet 5, core chat, Artifacts, Skills, Memory, file creation, up to 5 Projects |
| **Pro** | $20/mo | ~5x Free usage, Opus access, Research, Claude Code, Cowork, Design, Chrome, M365 |
| **Max 5x / 20x** | $100 / $200 mo | 5x or 20x Pro usage; Opus 5 default; Fable 5 included up to half your weekly limit |
| **Team** | Per seat (Standard / Premium) | Shared projects and skills, admin controls; Claude Code on Premium seats |
| **Enterprise** | Custom | Compliance, security, custom retention; usage billed at API rates on newer contracts |

**How limits actually work** (worth knowing precisely, because it shapes your day): two layers — a **session limit that resets every 5 hours** and a **weekly limit**. One pool covers everything: claude.ai, Cowork, Claude Code, Design. Watch it at Settings → Usage. Paid plans can enable **usage credits** (pay-as-you-go past the limit at API rates) or pre-buy discounted **usage bundles**.

The practical implication: heavy agentic work (Claude Code, Cowork) is what burns limits fastest, so pick effort and model deliberately, and don't leave an Opus-at-max-effort agent grinding on a task Sonnet would nail.

## 4. Data and privacy (know your settings)

- **Training:** on consumer plans, a single toggle at Settings → Privacy controls whether your chats can improve models; off means neither past nor future chats are used. Team, Enterprise, and API data is not trained on by default.
- **Retention:** deleted conversations leave your history immediately and are purged within 30 days. Enterprise can set custom retention.
- **Incognito chats:** available on all plans; not saved to history or memory, never used for training, auto-deleted.
- **One exception worth knowing:** Fable 5 and Mythos 5 are "Covered Models" — prompts and outputs are retained 30 days for safety monitoring on every platform, with no zero-retention option.

## 5. How the pieces fit

The mental model that makes everything else click:

- **Models** provide intelligence. You choose the tier and the effort.
- **Context** is what the model can see right now — the conversation, files, tool results. Finite, precious, and the thing you manage most.
- **Tools** are what it can do — read files, search the web, run code, call your APIs, click your browser.
- **Skills** are what it knows how to do *your way* — installed expertise (Module 01).
- **Memory / Projects / CLAUDE.md** are what persists between sessions.
- **Agents** are the loop: think → act → observe → repeat, until done.

Every product above is a different packaging of those six ingredients. Once you see them, a new Anthropic release stops being a mystery product and becomes an obvious recombination.

## 6. Exercises

1. Open Settings → Usage. Find your session and weekly meters. Check them again after a heavy Claude Code session; learn what burns your budget.
2. Audit your privacy settings deliberately: training toggle, memory, retention. Decide rather than default.
3. Write down the five recurring jobs in your week and assign each to a surface from §2. Keep the list; Module 08 uses it.

---

*Next: `01-skills.md` — start with the highest-leverage concept.*

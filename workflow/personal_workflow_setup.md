# 🧠 Personal Productivity System — Setup Guide

> For daily use, see the **Reference Card** instead.

A simple, low-friction system to manage tasks, knowledge, and daily focus — built around three tools:

* 📝 **Notebook** → thinking & focus
* ✅ **Todoist** → tasks
* 🧱 **Notion** → memory & context

---

# 📑 Table of Contents

1. [Methodology](#methodology)
2. [The 3 Components](#the-3-components)
3. [The Rule](#the-rule)
4. [Notion Structure](#notion-structure)
   - [Inbox](#inbox)
   - [Operations](#operations)   - [Projects → Topics](#projects--topics-db)
   - [Knowledge Base](#knowledge-base)
   - [Archive](#archive)
5. [How the System Works](#how-the-system-works-end-to-end)
6. [Practical Examples](#practical-examples)
7. [Todoist Setup](#todoist-setup-free-tier)
8. [Daily Routine](#daily-routine)
9. [Weekly Review](#weekly-review)
10. [Common Mistakes](#common-mistakes)
11. [System Summary](#system-summary)

---

# 🎯 Methodology

Most productivity systems fail not because people lack discipline, but because the system itself creates friction. When capturing a task takes 30 seconds of thinking about where it belongs, you stop capturing. When your notes are scattered across tools, you stop trusting the system. When Notion becomes a task manager, it slows you down.

This system is built around a single principle:

> **Every tool does one thing. No tool does everything.**

## The three roles

**Capture** happens in Todoist — fast, frictionless, available everywhere. The moment something appears, it goes in. No organising, no thinking, no deciding where it belongs yet.

**Focus** happens in the Notebook — a physical or digital scratchpad where you decide what matters today. The Big 3 (2–3 tasks maximum) gives the day a shape. Raw meeting notes live here too, unprocessed and messy. That is fine.

**Memory** lives in Notion — structured, linked, and built to last. Decisions, processed meeting notes, active initiatives, and reference knowledge all have a defined place. You go to Notion to remember and to understand context, not to manage tasks.

## Why this works

The system is inspired by *Getting Things Done (GTD)* but stripped down to its core loop:

```
Capture → Process → Work → Store
```

Capture anything immediately (Todoist or Notebook). Process it at a defined moment — not in the moment. Work from a short, deliberate list (Notebook Big 3). Store what matters for future reference (Notion).

The result: a clear mind during the day, a trusted place for everything, and no cognitive overhead about where things live.

---

# ✅ The 3 Components

## 📓 Notebook

👉 **Daily focus & thinking**

* Write the Big 3 each morning — the 2–3 tasks that matter today
* Take raw notes during meetings — messy is fine, it gets processed later
* Park ideas quickly without context-switching

❗ Not a task manager. Not a knowledge base. Just today.

---

## ✅ Todoist (Free)

👉 **Source of truth for all tasks**

* Every task, regardless of context, goes here first
* Due dates for time-sensitive work (free tier: date only, no time)
* Priorities (P1–P4) to triage without labels

❗ Don't work directly from Todoist during the day — use the Notebook Big 3 instead.

**Free tier limitations and workarounds:**

| Limitation | Workaround |
|------------|------------|
| No reminders | Use phone calendar for time-critical events |
| No deadline field (separate from due date) | Add to task title: `Fix login bug [by 15 May]` |
| No labels or filters | Keep to 2 projects max — context goes in the task title |

---

## 🧱 Notion

👉 **Memory, context & knowledge**

* Active initiatives tracked as Topics with status, dates, and risk
* Meeting notes processed and linked to the right Topic
* Long-form notes and documents stored with full context
* Reference knowledge that doesn't expire

❗ Not for tasks. Not for quick capture. Go here to remember, not to decide.

---

# ⭐ The Rule

```
Notebook → what I do now
Todoist  → everything I need to do
Notion   → what I need to remember
```

If something doesn't fit cleanly into one of these, it's a sign the system is being misused — not that the system needs more complexity.

---

# 🧱 Notion Structure

## Overview

```
Inbox

▾ Operations
    Meetings (DB)
    ... (additional meeting types or event logs as needed)

▾ Projects
    Topics (DB)

▾ Knowledge Base
    Tech Reference
    Processes
    Templates

Archive
    2025
    2026
```

---

## Inbox

Quick capture inside Notion — ideas, half-formed thoughts, anything without a clear home yet.

Process regularly (daily or every other day): move each item to the right place or discard it. The Inbox should never be a permanent home for anything.

**Where things go when processed:**

| Note type | Where |
|-----------|-------|
| Belongs to an active Topic | Topic scratchpad or sub-page |
| Technical reference, no Topic | Knowledge Base → Tech Reference |
| Process or workflow | Knowledge Base → Processes |
| No context yet | Leave in Inbox until context is clear |
| Not worth keeping | Discard |

---

## Operations

### Meetings (DB)

One database for all work meetings. Each meeting is a row.

**Properties:**
* `Meeting Date` (date)
* `Topic` (relation → Topics DB)
* `Type` (select: Recurrent | On Demand)

**Views:**
* `Recurrent` → filter: Type = Recurrent
* `By Topic` → group by: Topic

**Inside each meeting row:**
* Notes taken during or after the meeting
* Action items (checklist)
* Decisions made

❗ Recurrent meetings with no project context leave the Topic field empty.

---

### Additional Meeting Types

Not all meetings belong in the main Meetings DB. Some have a different cadence or audience that warrants a separate structure. Common examples:

* **1to1 Meetings** — belong to people, not initiatives. A separate DB with `Person` and `Date` works better than mixing them with project meetings.
* **Event logs** — conferences, offsites, company events. A simple page per event is enough; no DB needed.

The pattern is the same: if a meeting type has a distinct audience or purpose that doesn't fit the `Topic` relation, give it its own space rather than forcing it into the main Meetings DB.

---

## Projects → Topics (DB)

The central database. A Topic is any initiative, project, or area of active work.

**Properties:**
* `Status` (select: In Progress | To Do | Done | On Hold)
* `Date` (date range: start → end)
* `Start` (formula)
* `End` (formula)
* `Risk` (select: 🟢 🟠 🔴)
* `Tracker` (URL — external issue tracker, if applicable)

**Start formula:**
```
if(dateBetween(dateStart(prop("Date")), now(), "days") < 0,
   "Ongoing",
   if(dateBetween(dateStart(prop("Date")), now(), "days") == 0,
      "Starts today",
      "Starts in " + format(dateBetween(dateStart(prop("Date")), now(), "days")) + "d"))
```

**End formula:**
```
if(dateBetween(dateEnd(prop("Date")), now(), "days") < 0,
   "Ended " + format(abs(dateBetween(dateEnd(prop("Date")), now(), "days"))) + "d ago",
   "Ends in " + format(dateBetween(dateEnd(prop("Date")), now(), "days")) + "d")
```

**Views:**
* `Topics` → main view, filter: Status ≠ Done — active work only
* `Board status` → kanban grouped by Status
* `All` → no filter, full history including Done

Topics with Status = Done are never physically moved or deleted — they stay in the database. The `Topics` view filters them out automatically, keeping the working view clean while preserving all history, links, and meeting relations.

---

### Inside each Topic row

The body of every Topic row has two toggle sections and an inline scratchpad:

```
▶ 📅 Meetings
    └── linked view of Meetings DB
        filter: Topic = this Topic

▶ 📝 Notes
    └── sub-pages for long or structured notes

---
💬 Scratchpad
2026-05-07 — quick thought or observation
2026-05-08 — another short note
```

Opening a Topic gives the full picture — all meetings, all notes, and all quick thoughts in one place.

**Where does a note go?**

| Note type | Where |
|-----------|-------|
| 1–3 lines, no structure | Inline scratchpad — date prefix, append over time |
| Longer, structured, has a title | Sub-page inside the Topic row |

---

## Knowledge Base

Long-term reference material. Nothing actionable here.

* `Tech Reference` — technologies in use, architectural decisions, technical context
* `Processes` — how things are done: runbooks, retro formats, recurring workflows
* `Templates` — reusable page structures for Topics, meetings, notes

---

## Archive

For loose pages, old events, and documents that are no longer active — organised by year.

```
Archive
    2025
    2026
```

Move here: completed Major Events, outdated Knowledge Base pages, loose documents without a home.

**Topics are not moved to Archive** — they stay in the Topics DB with Status = Done, hidden from the main view by the filter. This preserves all meeting links and history.

---

# 🔁 How the System Works (end-to-end)

## Step 1 — Capture instantly

The moment something appears — a task, an idea, a thing to remember — capture it without thinking about where it belongs.

| What | Where |
|------|-------|
| Task or action | Todoist |
| Quick idea or thought | Notion Inbox or Notebook |
| Notes during a meeting | Notebook |

✔ No organising. ✔ No deciding. ✔ No friction.

---

## Step 2 — Process (once or twice a day)

At a defined moment — morning or end of day — go through what was captured.

**Todoist inbox:**
* Actionable → keep, add due date and priority
* Worth remembering but not actionable → move to Notion
* Neither → delete

**Notion inbox:**
* Belongs to a Topic → Topic scratchpad or sub-page
* Is reference knowledge → Knowledge Base
* Neither → discard

**Notebook notes from meetings:**
* Extract tasks → Todoist
* Extract decisions and context → relevant Topic in Notion

---

## Step 3 — Work during the day

* Pick the Big 3 in the morning and write them in the Notebook
* Work from the Notebook — not from Todoist, not from Notion
* Capture anything new in Todoist as it appears
* Take raw meeting notes in the Notebook

---

## Step 4 — After meetings

Ask: *"Is this something I need to act on or remember?"*

* **Act on** → create task in Todoist
* **Remember** → process into the relevant Topic in Notion

### Transformation example

**Raw (Notebook, during meeting):**
```
DB is slow under load
maybe indexes?
check slow query log → Sara
```

**Processed (Notion → Topic → scratchpad):**
```
2026-05-07 — DB performance sync
  Problem: high latency under load
  Decision: add indexes on most queried fields
  Next step: Sara checks slow query log
```

**Tasks (Todoist):**
```
Follow up with Sara on slow query log P2
```

---

# 📊 Practical Examples

## Example 1 — New initiative appears

A decision comes down to migrate the app to a different database engine.

1. Create a row in Topics DB: `DB engine migration`
2. Fill: Status = To Do, Date range, Tracker URL if available
3. First meeting → create row in Meetings DB, link Topic = `DB engine migration`
4. Initial notes → inline scratchpad of the Topic

---

## Example 2 — Recurring meeting with project context

Weekly sync on an ongoing API redesign.

1. Raw notes in Notebook during the meeting
2. After: open the meeting row in Meetings DB (Type = Recurrent, Topic = `API redesign`)
3. Write processed notes inside the meeting row
4. Tasks extracted → Todoist: `Update auth endpoint documentation P2`
5. Decisions → Topic scratchpad: `2026-05-07 — agreed to deprecate v1 endpoints by end of Q2`

---

## Example 3 — Note or idea mid-work

Working on a CI/CD pipeline improvement, something comes to mind.

* Short thought → scratchpad: `2026-05-07 — consider splitting build and deploy stages`
* Longer analysis with structure → sub-page inside the Topic row: `Build vs deploy separation — options and trade-offs`

---

# ⚙️ Todoist Setup (Free tier)

**Projects (2 max):**
* Work
* Personal

❗ Don't create a project per Topic — use the task title for context:
`[API redesign] Update auth endpoint docs` tells you everything without a dedicated project.

**Priorities:**
* P1 → urgent, must do today
* P2 → important, do this week
* P3 → nice to have, no deadline pressure
* P4 → someday / maybe

**Due dates:**
* Free tier: date only, no time
* For time-critical appointments → use phone calendar instead

**Deadlines:**
* No separate deadline field on free — encode it in the title:
  `Submit architecture proposal [by 20 May]`

---

# 📓 Daily Routine

## Morning
* Review Todoist — scan what's due and overdue
* Pick the Big 3 — write them in the Notebook
* Glance at Topics DB — anything ending soon? (End formula)

## During the day
* Work from the Notebook Big 3
* Capture new tasks immediately in Todoist
* Take raw meeting notes in the Notebook

## End of day
* Process the Todoist inbox
* Process Notebook meeting notes → extract tasks to Todoist, context to Notion
* Clear the Notion Inbox

---

# 🔁 Weekly Review

Once a week — Friday or Monday — go through the system:

* **Topics DB** — any Topic with Status = Done for more than 2 weeks? Mark it Done and confirm it's filtered out of the main view
* **On Hold Topics** — still relevant? Resume or mark Done
* **Notion Inbox** — anything left unprocessed from the week?
* **Todoist** — any P3/P4 tasks that have been sitting too long? Delete or reschedule
* **Archive** — move any loose old pages or events to the correct year folder

This is the step that prevents entropy. Without it, the system slowly fills with stale content and stops being trustworthy.

---

# ⚠️ Common Mistakes

| Mistake | Why it breaks |
|---------|---------------|
| Using Notion for task management | Too slow for daily task work — use Todoist |
| Working directly from Todoist during the day | Too many tasks visible — use the Notebook Big 3 |
| Creating meeting rows without linking to a Topic | Notes become orphaned and unsearchable |
| Writing long structured notes in the inline scratchpad | Hard to navigate — use a sub-page instead |
| Moving Done Topics to Archive as pages | Breaks all meeting and note relations — keep them in the DB |
| Storing everything in Notion | Notion becomes a dump, not a memory system |
| Creating a Todoist project per Topic | Hits free tier limits and adds friction |
| Skipping the processing step | Inbox grows, system stops being trusted |

---

# 🧩 System Summary

```
Capture  → Todoist (tasks) + Notion Inbox (ideas) + Notebook (during meetings)
Focus    → Notebook (Big 3)
Remember → Notion Topics + Knowledge Base
```

---

# 📌 Final Principle

> Keep capture fast.
> Keep execution simple.
> Keep knowledge structured.

The system only works if you use it every day. Start with the minimum — one week of consistent use — before adding anything new.

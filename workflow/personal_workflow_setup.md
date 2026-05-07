# 🧠 Personal Productivity System — Setup Guide

> For daily use, see the **Reference Card** instead.

---

# 📑 Table of Contents

1. [Methodology](#methodology)
2. [The 3 Components](#the-3-components)
3. [The Rule](#the-rule)
4. [Notion — Work Space](#notion-work-space)
   - [Inbox](#inbox)
   - [Operations](#operations)
   - [Projects → Topics](#projects--topics-db)
   - [Knowledge Base](#knowledge-base)
   - [Archive](#archive)
5. [Notion — Personal Space](#notion-personal-space)
6. [Todoist Setup](#todoist-setup-free-tier)
7. [Common Mistakes](#common-mistakes)

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

# 🧱 Notion — Work Space

## Overview

```
Inbox

▾ Operations
    Meetings (DB)
    ... (additional meeting types as needed)

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

Quick capture inside Notion — ideas, half-formed thoughts, anything without a clear home yet. Process regularly: move each item to the right place or discard it.

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

**Why one DB instead of multiple:** a single database with a `Type` property and filtered views covers all meeting categories without fragmenting the data. If a meeting type has a fundamentally different audience or purpose — such as 1to1s, which belong to people rather than initiatives — it warrants its own separate database.

---

## Projects → Topics (DB)

The central database. A Topic is any initiative, project, or area of active work — technical migrations, process improvements, vendor evaluations, anything with a beginning, an end, and things to track.

**Properties:**
* `Status` (select: In Progress | To Do | Done | On Hold)
* `Date` (date range: start → end)
* `Start` (formula — see below)
* `End` (formula — see below)
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

**Why Done Topics stay in the DB:** moving them out as pages breaks all meeting and note relations. Status = Done + filter on the main view keeps the working view clean while preserving full history and links.

---

### Inside each Topic row

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

**Where does a note go?**

| Note type | Where |
|-----------|-------|
| 1–3 lines, no structure | Inline scratchpad — date prefix, append over time |
| Longer, structured, has a title | Sub-page inside the Topic row |

**Why no separate Notes DB:** for personal use, sub-pages inside the Topic row are sufficient and have zero setup overhead. A Notes DB only adds value if you need to query or filter notes across all Topics — which is rare in a personal system.

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

**Topics are not moved here** — they stay in the Topics DB with Status = Done, filtered out of the main view. This preserves all relations. Archive is only for content that lives outside the databases: old event pages, outdated Knowledge Base pages, loose documents.

---

# 🏡 Notion — Personal Space

A separate Notion space for personal life — not work. Simpler structure, no databases unless volume or filtering genuinely justifies one.

## Overview

```
Inbox

▾ Life Management
    Family
    Casa
    Saúde
    Cars

▾ Food & Drinks
    Restaurantes
    Pratos para cozinhar
    Take away
    Café

▾ Knowledge
    Tech
    AI
    Photography
    Management
    Books

▾ Reference
    Websites úteis
    Checklists
    Viagens - o que levar

Archive
    2025
    2026
```

---

## When to use a database vs simple pages

The default is simple pages. A database is only worth the setup overhead if at least one of these is true:

* You have enough entries to need filtering or sorting (rough threshold: 10+)
* You need to relate entries to each other or to another database
* You need properties beyond a title and free text

For small, stable collections — 4 family members, a handful of restaurants, a few saved recipes — simple pages are faster to create, faster to read, and require zero maintenance.

---

## Section purposes

**Life Management** — everything needed to manage real life and ongoing responsibilities. If this information disappeared, day-to-day life would be harder to run. Contacts, contracts, medical history, car records.
*Not here: hobbies, learning, bookmarks, food.*

**Food & Drinks** — culinary and gastronomic reference. Consulted regularly when deciding what to cook or where to eat.
*Not here: nutrition articles, generic shopping lists.*

**Knowledge** — knowledge acquired and worth retaining. Technical, professional, intellectual. Your personal reference library.
*Not here: links not yet read, operational personal info, to-learn tasks.*

**Reference** — utility information consulted occasionally that doesn't belong anywhere else. If it clearly belongs in another section, put it there instead.
*Not here: knowledge → Knowledge, personal info → Life Management, food → Food & Drinks.*

**Archive** — content no longer active but not worth deleting. Organised by year. If consulted in the last 3 months, it doesn't belong here.

---

**Projects (2 max):**
* Work
* Personal

❗ Don't create a project per Topic — context goes in the task title:
`[API redesign] Update auth endpoint docs`

**Priorities:**
* P1 → urgent, must do today
* P2 → important, do this week
* P3 → nice to have, no deadline pressure
* P4 → someday / maybe

**Free tier limitations and workarounds:**

| Limitation | Workaround |
|------------|------------|
| No reminders | Use phone calendar for time-critical events |
| No deadline field | Add to task title: `Fix login bug [by 15 May]` |
| No labels or filters | Keep to 2 projects max — context in task title |

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

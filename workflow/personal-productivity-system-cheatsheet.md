# Cheatsheet — Quick Reference

A condensed, scannable companion to `personal-productivity-system.md`. Use this when you already know the system and just need a quick answer — go back to the full document for the reasoning behind any of it.

---

## Where does this go?

```mermaid
flowchart TD
    A[Something needs your attention] --> B{What kind of thing is it?}
    B -->|A concrete action| Todoist[("📋 Todoist")]
    B -->|"Today's focus"| Notebook[("📓 Paper notebook")]
    B -->|"Reference, history, or structure"| Notion[("🗂️ Notion")]
```

| If it's... | It goes to |
|---|---|
| Something to *do* | Todoist |
| Today's top 3 focus | Notebook |
| Reference, history, or something to compare against others | Notion |
| Not committed to yet, just an idea | Notion → Someday / Ideas |
| Still matters but didn't happen today | Notebook `✕` → Todoist |

---

## Todoist

| Element | Values |
|---|---|
| Projects (top-level tags) | `#Work` · `#Personal` |
| Labels | `@waiting` · `@quick` · `@email` |
| Optional label | `@deepwork` — only if it changes what you actually do |
| Saved filters | `Today` · `Next 7 days` · `@waiting` |
| Naming rule | **Always start with a verb** — "Schedule X," not "X" |

GTD loop this maps to: **Capture** (quick-add) → **Clarify** (verb-first naming) → **Organize** (tags + labels) → **Reflect** (rituals) → **Engage** (saved filters).

---

## Notebook

| Symbol | Meaning | When |
|---|---|---|
| `□` | Not done yet | Morning |
| `✓` | Done | Evening |
| `→` | Still matters, not today → goes to Todoist | Evening |
| `✕` | Cancelled, no longer needed | Evening |

- Big 3 only. Written each morning.
- No index, no migration, no weekly review of old pages — every page dies at midnight.
- Optional: one line drafting tomorrow's first item, at day's end. Skip it whenever it doesn't come naturally.

---

## PARA (Notion)

```mermaid
flowchart LR
    P["🗂️ Projects<br/>most actionable"] --> A["🟩 Areas<br/>ongoing, no deadline"] --> R["🟨 Resources<br/>low actionability"] --> AR["📦 Archive<br/>inactive"]
```

| Category | Rule of thumb |
|---|---|
| **Projects** | Has an end. Multiple sessions of work. |
| **Areas** | Ongoing. Never ends. |
| **Resources** | Reference. No responsibility attached. |
| **Archive** | Inactive. Never deleted. |

> **Golden rule:** never create an empty folder, tag, or database before you have real content for it.

---

## Database or Page?

| Question | → Page | → Database |
|---|---|---|
| How many items? | Few, stable (≤ ~8–10) | Many, or growing |
| Same shape (same fields)? | No | Yes |
| Need to filter/sort/compare? | No | Yes |
| Repeats often? | No | Yes |

Two or more answers on one side decide it. Tie → keep what you already have.

| Property choice | Use it when... |
|---|---|
| **Select** | One exclusive value from a fixed list (Status, Stage) |
| **Multi-select (Tags)** | Zero, one, or many loose labels — no forced category |
| **Relation** | A genuine two-way link, or a value duplicated across 3+ databases |
| **Date (manual)** | A deliberate human signal (e.g. "I reviewed this") |
| **Created / Last edited time (built-in)** | Free, automatic — use instead of a manual date whenever editing *is* the update |

---

## The Notion structure

```mermaid
graph TD
    Sidebar --> Projects[("🗂️ Projects (DB)")]
    Sidebar --> Areas["🟩 Areas"]
    Sidebar --> Resources["🟨 Resources"]
    Sidebar --> Archive["📦 Archive"]
    Areas --> PM["People Management"]
    Areas --> Teams[("⭐ Teams (DB)")]
    Areas --> Meetings[("⭐ Meetings (DB)")]
    Areas --> SC[("⭐ Service Catalog (DB)")]
    Areas --> RM[("⭐ Roadmap (DB)")]
    PM --> Persons[("⭐ Persons (DB)")]
    PM --> OneOnOne[("⭐ 1:1 Meetings (DB)")]
    Resources --> SI[("Someday / Ideas (DB)")]
```

---

## All databases, at a glance

| Database | Lives in | ⭐ Favorited | Key fields | One-liner |
|---|---|---|---|---|
| **Projects** | Top level, alone | — (it IS a top-level category) | Status, Risk, Team, Target Date | Anything with an end date |
| **Teams** | Areas, direct | ✓ | Name only | The hub everything else relates to |
| **Persons** | Areas → People Management | ✓ | Team, Role, Last 1:1 | Who you work with |
| **1:1 Meetings** | Areas → People Management | ✓ | Team | One row per person, log grows forever |
| **Meetings** | Areas, direct | ✓ | Type, Project, Team | Recurring + one-off, together |
| **Service Catalog** | Areas, direct | ✓ | Team, Criticality, Status, Last Reviewed | Living memory of systems you own |
| **Roadmap** | Areas, direct | ✓ | Year, Status, Theme, Team | One DB, every year, never recreated |
| **Someday / Ideas** | Resources | ✗ (checked occasionally, on purpose) | Tags | Loose ideas, not yet committed |

---

## Closing things out (Archive, in practice)

| Thing | How it "archives" |
|---|---|
| A **Project** | `Status → Done`. Stays in the DB. Split into two views: active / done. |
| A **Service Catalog** entry | `Status → Deprecated` or `Being Phased Out`. Stays in the DB. |
| A **Someday/Ideas** row | Graduates into a Project, then **gets deleted**. No record kept — the Project is the record now. |
| Anything living as a **plain page** (no Status field) | Physically moved to the **Archive** folder. |

Rule: if it's in a database with a Status field, change the status — don't move the row. Archive-the-folder is only for things that don't have a status of their own to retire into.

---

## Daily & weekly ritual

```mermaid
flowchart LR
    M["🌅 Morning — 2 min"] --> D["☀️ During the day"] --> E["🌙 End of day — 2 min"] --> F["📅 Friday — 15 min"]
    F -.-> M
```

| When | Do |
|---|---|
| Morning | Todoist "Today" filter → pick the notebook's Big 3 |
| During the day | Mark symbols as tasks close |
| End of day | Close symbols (`✓`/`→`/`✕`) · `→` items go to Todoist · optionally draft tomorrow |
| Friday | Notion: clear Inbox → update Project statuses → check 1–2 Service Catalog entries |

---

## Designing any new database

1. Few properties — only what you'll actually filter or sort by.
2. Right type per field: Select (fixed), Relation (only for real 2-way links or 3+ duplication), Date (only real dates).
3. Title identifies the row on its own.
4. Rich content → page body, never a property.
5. 2–3 views cover almost everything: filtered daily view, grouped overview, unfiltered audit.
6. Use built-in Created/Last edited time before adding a manual date field.

If rows start needing very different fields from each other → split into two databases, or it shouldn't be a database at all.

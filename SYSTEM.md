# HQ — Personal Operating System
_Last updated: 2026-06-18_

---

## The four layers

| Layer | What belongs here | Folder |
|---|---|---|
| **Reference** | Notes, articles, contracts, SOPs, decisions, templates, research, client context, ideas | `reference/` |
| **Projects** | Anything with an outcome that takes more than one action | `projects/` |
| **Actions** | Single executable next steps | `actions/` |
| **Calendar** | Anything tied to a specific date or deadline | `calendar/` |

**Rule:** A note is not a task. A task is not a project. A project is not a calendar event. When in doubt, use the type definitions above.

---

## Capture → Sort workflow

1. **Capture** — drop everything into `INBOX.md` first. Don't sort while capturing.
2. **Sort** — daily, move each inbox item to its correct layer:
   - Has a date? → `calendar/important-dates.md` or task due date
   - Has an outcome? → `projects/`
   - Single executable step? → `actions/`
   - Need it later but no action? → `reference/`
   - About a client/person? → `clients/`
   - Money in or out? → `finance/tracker.md`
3. **Link** — every project/action should reference its client, project, or source
4. **Review** — daily for actions, weekly for projects + calendar

---

## Project anatomy

Every project file must have:
- **Outcome** — one sentence, what done looks like
- **Status** — `active | paused | waiting | done`
- **Next action** — the one thing to do next
- **Owner** — who is responsible
- **Deadline** — date or "none"
- **Category** — from shared categories below
- **Related** — links to clients, reference files, finance items

---

## Action anatomy

Every action must have:
- **Verb** — starts with a verb (Call, Write, Review, Send, Fix, Pay)
- **Project/Client** — context it belongs to
- **Status** — `todo | in-progress | waiting | done`
- **Priority** — `high | normal | low`
- **Due** — date or "none"

---

## Shared categories

`Client` `Project` `Finance` `Legal` `Marketing` `Operations` `Personal` `Reference` `Idea` `SOP` `Decision` `Admin` `Follow-up`

---

## Role map

| Tool | Role |
|---|---|
| **Hermes** | Brand, offers, positioning, copy, creative direction |
| **Claude** | Strategy, planning, reasoning, critique, synthesis |
| **Claude Code** | Code, repos, terminal, file-based implementation |
| **Codex** | Local files, spreadsheets, automation scaffolding |
| **OpenClaw** | Reminders, follow-ups, scheduling, monitoring, comms |

### Handoff format
When passing work between tools, include:
- Objective
- Current state
- Decisions made
- Open questions
- Next actions
- Relevant files/links

---

## Weekly review checklist

- [ ] Inbox cleared
- [ ] All active projects have a next action
- [ ] Calendar/important dates checked for the next 14 days
- [ ] Finance tracker current
- [ ] Stale actions (>7 days old) reviewed — kill or reschedule
- [ ] PLAYBOOK.md in SingleBrain updated (marketing rhythm)

---

## Folder index

```
HQ/
├── INBOX.md              — capture first, sort daily
├── SYSTEM.md             — this file
├── projects/             — one file per project
│   └── _template.md
├── actions/              — one file per category or running list
│   └── _template.md
├── reference/            — one file per topic/client/type
│   └── _template.md
├── calendar/
│   └── important-dates.md
├── clients/              — one file per client/person
│   └── _template.md
└── finance/
    └── tracker.md
```

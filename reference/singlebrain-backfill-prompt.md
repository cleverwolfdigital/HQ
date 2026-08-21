# Prompt — Backfill All Projects into Single Brain

**Type:** SOP
**Category:** Operations
**Source:** Built 2026-08-21 after adding Learning to Learn to the dashboard
**Date:** 2026-08-21
**Related:** [[SYSTEM.md]] · [[PORTFOLIO.md]] · brain.cleverwolfdigital.com

---

## What this is

A paste-ready prompt for a future Claude Code session. It populates the Single
Brain Projects page with every live project, so the dashboard shows the real
picture instead of a stale one.

**Why it needs to be a prompt and not a script.** The hard part is not writing
rows — it is knowing what is actually true right now. Memory notes and READMEs
drift; a project recorded as "blocked on the client" may have shipped weeks ago.
So the prompt's main job is to force verification against live systems *before*
anything is written. A script that dumped my notes into the database would give
you a tidy dashboard full of confident, wrong statements — worse than an empty
one, because you would trust it.

Expect it to take a while. That is the point.

## Before you run it

As of 2026-08-21 the dashboard holds **30 businesses, 24 clients, 10 projects**.
The businesses and clients are in good shape. Projects are the gap.

Run it when you have time to answer a few mapping questions — the prompt is
told to ask rather than guess where a project belongs.

---

## The prompt — copy everything below

```
Populate the Single Brain dashboard with all of my current projects, so the
Projects page reflects reality rather than a stale snapshot.

## The rule that matters most

Do NOT trust your memory files, my HQ notes, or any README as a statement of
current status. Treat them ONLY as a list of candidates worth investigating.
For every project, verify the real state against live systems before writing
anything — check the site, the server, the repo, the database, the mailbox,
whatever is authoritative for that project.

If you cannot verify something, say so and mark it clearly. A project written
in as "Active — awaiting client reply" when it actually shipped last month is
worse than no row at all, because I will believe it.

Where a project's real state contradicts what a note claims, tell me. Those
contradictions are the most valuable output of this whole exercise.

## Where things live

- Dashboard: https://brain.cleverwolfdigital.com (I am usually logged in)
- SSH alias `hostinger`
- Live DB: run `sb-db` on the server. Do not hardcode a path — sb-db derives it
  from systemd and confirms it against the running process's open files, so it
  cannot go stale. `sb-db -v` shows its working; `sb-db -q` opens a sqlite shell.
- Back the DB up before the first write:
  DB=$(sb-db)
  sqlite3 "$DB" ".backup /root/backups/singlebrain/$(date +%Y-%m-%d-%H%M)/singlebrain.db"

Write SQLite directly using the same columns as POST /api/projects — that
endpoint needs an admin session cookie, so direct SQL is the practical route
and produces an identical row. Upsert on (name, business) so the work is
repeatable and never duplicates.

A working example to copy the shape from:
C:\Users\tidas\lus-learning-to-learn\tools\singlebrain_status.sh

## Field vocabulary — use ONLY these

The frontend styles these exact values and renders anything else as unstyled
text, so inventing a status silently degrades the page.

  status : active | live | parked
  state  : Active | In Progress | Not Started | Scheduled | Partial
  badge  : warning | parked | ''      (empty means no flag)
  due    : a short date like 'Sep 15', or '—'
  kind   : project | campaign

resume_prompt is the other column that matters. It holds a prompt that resumes
work on that project, and the dashboard puts a Resume button on any card that
has one — click it and you get the prompt in a dialog with a copy button. Write
one for every project you add. Status tells me where something stands; the
resume prompt is what stops the baton being dropped.

A good resume prompt states where the project stands with dates and real IDs,
where the code and the server are, what has been verified and how to re-verify
it, what is genuinely open, and the traps that already cost time. Assume the
reader has no memory of the project at all — that is the situation it exists
for. Two thousand characters is a reasonable size; a one-liner is not worth
storing.

Set badge='warning' when there is a genuine open risk or blocker, and put the
actual blocker in next_action — specific and actionable, not "continue work".
The whole value of this page is that a flag means something.

Business must match a row in the businesses table exactly. Client work belongs
to the client's business (usually Clever Wolf Digital); there is a separate
clients table already populated with 24 clients — link to it where the schema
allows rather than inventing a business.

## Candidates to investigate

Start from my memory index and HQ/PORTFOLIO.md. Known active areas as of
2026-08-21, all of which need their real status checked:

  Level Up Smarter   — Learning to Learn course (already in, id 16), content
                       engine, video pipeline, site/hero images, plateau article
  Level Up Self      — supplements catalogue, research glossary
  CleverWolf.ai      — CWD Sprint funnel, CFDE training programme
  Clever Wolf Digital— Single Brain itself, Calendly funnel, Game Grader
  Client work        — ChaneyBrooks, Min Plastics, My Goodnight Prayers,
                       Get Onward, Lex Brodies, Kimchee 88, Anker/Battery Bill,
                       Novelbound, Opala Hoa, Kapwa reporting, Forty Plus
  The Smarter Podcast— SEO project

This list is neither complete nor authoritative. Some of these are finished,
some are dormant, some may not deserve a row. Deciding which is part of the job.

## How to work

1. Read the current businesses, clients and projects tables first, so you match
   existing conventions and do not duplicate what is already there.
2. Verify each candidate against live systems. Batch the independent checks.
3. Decide what earns a row. A finished piece of work does not need one; an
   ongoing commitment does. Tell me what you dropped and why.
4. Ask me about any project whose business mapping is genuinely ambiguous —
   do not guess. Group your questions and ask once, not one at a time.
5. Back up, then write.
6. Verify on the RENDERED Projects page, not just the row. The Overview widget
   shows only the first six projects (.slice(0, 6)), so scraping it right after
   a write makes a perfectly good row look like it failed to save. Click through
   to Projects and confirm the cards.
7. Write a resume_prompt for every project you add, to the standard above.
8. Report: what you added, what you skipped, what contradicted my notes, and
   anything you could not verify.
```

---

## Notes for future me

- The `warning` badge is the most useful field on the page. Resist the urge to
  flag everything — if half the cards are amber, none of them mean anything.
- `next_action` is what makes the page worth opening. "Ongoing operations" is
  filler; "Verify WP Swings renewals fire before the $49/mo plan opens" is not.
- A stale resume prompt is worse than none, because it will be trusted. Refresh
  it in the same session that changes the project.
- Re-running the prompt later is safe by design — the upsert refreshes each row
  rather than duplicating it.
- Per-session status updates are already a standing practice, so this backfill
  should only ever be needed once. After it, each project's status gets pushed
  at the end of the session that touches it.

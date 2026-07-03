---
name: morning-briefing
description: Five-part morning pipeline, calendar conflicts and no-agenda meetings, tasks due or overdue, goal/KPI drift, a quick Tier-1-only inbox scan, then one formatted CALENDAR/TASKS/GOALS/URGENT/FOCUS brief ending in a single focus line. Run every morning before the founder's/exec's day starts, or on request for a quick state-of-play.
user_invocable: true
---

# Morning Briefing

One short, scannable brief so the founder or exec doesn't have to reconstruct their own day, or their own open loops, from memory. Five checks, in order: calendar, tasks, goals, a quick urgent scan, then one printed brief. Its job is to catch what's about to become a problem before it does, while there's still time to head it off.

## Operating Rules (read first)

- **This is a brief, not a report.** If nothing needs action today, say that in one line and stop. Don't manufacture urgency to fill space.
- **Never invent a calendar event, a due date, or an interaction.** If it isn't in `context/`, in what the user pasted in, or returned by a live integration, don't assert it happened.
- **Read `context/people/*.md` and `context/decisions-log.md` fresh every run.** Don't lean on what yesterday's brief said, files may have changed.
- **Cadence decides staleness, not a fixed day-count.** A daily-working relationship going quiet for a week is a different signal than a monthly-board-update relationship going quiet for a week.
- **Disagreement between files is itself a finding.** If a person's file lists something as open but the decisions log shows it closed (or the reverse), don't silently pick one. Surface it.
- **The Step 5 scan is Tier-1 only, on purpose.** This skill is not a substitute for a full inbox pass. If a real triage is needed, point to the separate skill built for that (see Intended Integrations).
- **This skill doesn't own KPI tracking.** If `okr-kpi-tracker` has produced a dashboard file, read it and fold in the drift. If not, pull drift signals from `context/goals.md` and any open item that explicitly names a metric.

## Intended Integrations (not live in this demo)

- **Google Calendar MCP** would power Step 2. Instead of waiting for the user to paste today's agenda, it would pull the real calendar, so conflicts, back-to-backs, and no-agenda meetings get caught automatically every morning, not only when someone remembers to hand over the day.
- **Gmail MCP** would power Step 5. It would do a quick, time-boxed read for Tier-1/urgent-only items, the kind of thing that should change what gets flagged this morning. This is deliberately not a full inbox triage, that job belongs to a separate skill (`inbox-triage`) built to do the deeper pass.
- Neither is wired into this repo. This is a demo of the operating logic, not a live deployment. Right now Steps 2 and 5 run on whatever the user pastes in, or come up empty and say so. Everything downstream, the flagging rules, the cross-file consistency check, the briefing format, stays the same whether the input is pasted text or a live MCP call.

## Step 1: Time and Date Context

Note today's date and day of week. Read `context/role-target.md` for who's being supported, the company, and current priorities. Every relative-date judgment downstream (overdue vs. due-today vs. approaching, which relationship cadence counts as stale) is anchored to this.

## Step 2: Pull Today's Calendar

If the user has pasted today's calendar or agenda, take it as given. Flag: **conflicts** (double-bookings), **back-to-backs** with no gap between them, and any meeting with **no stated agenda** and no context in `context/people/` for who it's with.

There's no live calendar integration in this system (see Intended Integrations above), so don't guess at meetings that haven't been provided. If none was given, say so in the CALENDAR section and move on. Only circle back to it later if something from Step 3 or 4 (a stale stakeholder, an overdue item) looks like it would change based on who's actually in today's meetings.

## Step 3: Check Tasks and Commitments

- Read `context/decisions-log.md`. Collect every entry with `Status: Open`.
- Read every file in `context/people/`. Collect each person's `Open items` section.
- **Cross-check the two sources against each other.** An item that's `Open` in a person's file but `Closed` in the decisions log (or the reverse) means one of the files didn't get updated after the fact. Flag it as a memory-hygiene gap, separate from a live risk.
- Sort what's left into **overdue** (past a stated date), **due today or this week**, and **open, no date given**.
- **Check staleness by cadence.** For each person in `context/people/`, compare their most recent `Interaction log` date against today, relative to the cadence stated in that file's `Relationship` line (daily, weekly, monthly, ad hoc). The thresholds are owned by `relationship-crm`'s tier table, use those, don't re-judge them here: daily-cadence stale past 14 days, weekly past 30, monthly past 60. Quiet inside the threshold isn't stale, but a missed sync against a stated cadence is still worth a line if something's riding on that person.
- **The sharpest signal is an open item sitting on a stale contact.** Someone is waiting on something and has gone quiet. Rank these above a plain overdue item with no relationship signal attached. Don't flag someone as stale just because their file is untouched, if there's no open item and the cadence is naturally long, quiet is normal.

## Step 4: Check Goal Alignment

Compare this week's open items and decisions against `context/goals.md`'s standing priorities. If something in play runs against one of them (a project gone quiet, a relationship going cold), that's a drift flag, not just a status line.

If an open item explicitly names a metric or a KPI review, pull it into the brief as-is. Don't derive or estimate a number that isn't in the source file. If a structured KPI/OKR dashboard file exists from `okr-kpi-tracker`, read it and merge its flagged drift in here. If it doesn't exist yet, say so once, don't re-flag its absence every morning.

## Step 5: Quick Urgent Scan (Tier-1 Only)

Do a light pass for anything that would change what "urgent" means this morning: a blocking question, a fire, something that can't wait for the rest of the pipeline above. This is not a full inbox triage, don't go looking for everything, only what's clearly Tier-1.

There's no live inbox integration in this system. In practice this step runs on whatever the user has already flagged in conversation, plus anything Steps 3 and 4 surfaced that's genuinely urgent (an overdue item that just became time-critical, a stale contact sitting on a live risk). If nothing qualifies, say so plainly, don't manufacture a fire to fill the section.

## Step 6: Print the Briefing

Compress everything above into one brief, in this order:

```
Morning brief — [Day, Date]

CALENDAR
[today's events with flags, or "no calendar provided today"]

TASKS
[overdue / due-today items, stale-relationship-with-open-item flags,
memory-hygiene gaps, ranked by risk]

GOALS
[drift flags against standing priorities, or "on track" if genuinely fine]

URGENT
[Tier-1 items from Step 5, or "nothing urgent surfaced"]

FOCUS RECOMMENDATION
[one line: what to focus on first, and why]
```

Rank within each section by risk, not by which file it came from. Compress a section down to one line if it's genuinely fine, don't pad it to look thorough. The FOCUS RECOMMENDATION line is mandatory even on a quiet day, it's the one thing the founder should read if they read nothing else.

## Worked Example (Nova Loop, fictional)

Say today is Thu 2026-07-02. No calendar was pasted in this morning. Reading the seeded example files:

```
Morning brief — Thu 2026-07-02

CALENDAR
No calendar provided today. If Sam or Priya turn up on today's calendar,
move the TASKS items below ahead of that meeting.

TASKS
1. Priya's Q2 KPI drift writeup was due before last Friday's board prep
   (2026-06-26), and the support-ticket-spike root cause it depends on is
   still open (logged 2026-06-24). Both slipped past that date, and the
   writeup can't be clean until the root cause is back. Chase the root
   cause first.
2. The relaunch-date decision (Sam's item, logged 2026-06-22) was due "by
   end of week." That week has passed, this is now overdue, not just open.
3. Missed cadence, open item attached: Sam Okafor, last logged contact
   2026-06-22 (10 days against a weekly sync, so a sync got skipped; not
   stale by relationship-crm's 30-day tier, but he's holding what item 2
   is stuck on). He's the one who flagged the support/CS gap. Worth a
   direct check-in today, not just a wait.
4. Memory-hygiene gap: Elliot's people file still lists the runway-scenario
   doc as an open item (logged 2026-06-18), but the decisions log shows it
   was delivered and closed on 2026-06-24. Close it in his file so it
   stops showing as open.

GOALS
Priya's KPI writeup and the relaunch decision both sit on standing
priority #2 (never let a special project stall silently) and #3 (keep the
relationship map current). Both are now past their stated dates, that's
drift already landed, not approaching.

URGENT
Nothing surfaced. No live inbox integration in this demo, and nothing in
today's TASKS or GOALS check crossed into Tier-1.

FOCUS RECOMMENDATION
Check in with Sam before anything else. He's holding the relaunch decision
and the ticket-spike root cause both, and he's gone quiet. Everything else
today is downstream of what he says.
```

## Rules

- Never fabricate a due date, a meeting, a conversation, or an inbox item. Missing information is a gap to surface, not a blank to fill in.
- A quiet stakeholder with no open item isn't a finding, don't flag noise.
- Always distinguish a live risk from a stale file. They need different actions, don't merge them into one line.
- If two source files disagree, say so plainly, don't quietly trust the more recent-looking one.
- The Step 5 scan stays Tier-1 only. If it starts trying to be a full inbox pass, that's scope creep, point to `inbox-triage` instead.
- If there's genuinely nothing urgent, the brief can be five short sections and a one-line focus recommendation. Don't pad it to look useful.

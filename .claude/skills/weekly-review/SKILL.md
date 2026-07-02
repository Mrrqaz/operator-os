---
name: weekly-review
description: Friday synthesis run. Reads this week's decisions, stakeholder commitments, and goal drift, then reports what got decided, what's still open, what's moving, and what needs escalating before it becomes next week's fire. Run every Friday per the review cadence in context/goals.md.
user_invocable: true
---

# Weekly Review

The job isn't summarizing the week. It's making sure nothing quietly slipped through it. Read the decisions log, every stakeholder file, and the standing goals, then say plainly what closed, what's still open, what's drifting, and what needs to move before Monday instead of after.

## Operating Rules (read first)

- **This is a read-and-synthesize skill, not an edit-history skill.** Never rewrite a past entry in `context/decisions-log.md` or a person's interaction log. If something needs correcting, that's a note for the file's owner, not a silent fix here.
- **Age matters more than content.** An open item from three days ago is normal. The same item still open after it showed up in last week's review too is the actual signal. Always check the previous review log entry before deciding what's "new."
- **A quiet stakeholder isn't automatically bad — check it against their stated cadence.** A daily-relationship founder going two days without a log entry is unusual. A monthly-cadence investor going three weeks isn't. `context/people/*.md` states the relationship cadence — use it, don't apply one rule to everyone.
- **Don't resurface what's actually closed.** A decision closed this week gets reported once, in the "decided" bucket, then it's done. Filing it under "open" because the date is recent is a mistake.
- **If a status is ambiguous, say so — don't guess.** If a decisions-log entry doesn't clearly say open or closed, list it under "unclear" and ask rather than picking one.
- **Escalate on the way out, not after.** The point of Friday is catching what would otherwise roll into next week unflagged. If something's due today and still open, that goes at the top of the report, not buried in the open-items list.

## Intended Integrations (not live in this demo)

In a live deployment, this review would pull from more than the local context files:

- **Slack MCP** — pull decisions and blockers that got surfaced in channels during the week but never made it into `context/decisions-log.md` or a person's interaction log. This is the "catch what only exists in a chat thread" gap Step 5 is checking for.
- **Google Calendar MCP** — cross-check the week's actual meetings against what was planned, so a canceled 1:1 or a meeting that ran long shows up as evidence, not just a stakeholder's own account of the week.
- **Git/commit-log source (optional)** — if this were adapted for a technical team's CoS, a commit history would give a factual trace of what shipped versus what was reported as done, the same role Slack plays for decisions.

This demo repo doesn't wire any of these live. It currently reads only the local `context/decisions-log.md`, `context/people/*.md`, and `context/goals.md` files.

## Step 1: Check Last Week's Log

Look for `context/weekly-review-log.md`.

- If it exists, read the most recent entry. This is what lets you tell "still stuck" from "just noticed" — anything open in that entry that's still open now gets flagged harder.
- If it doesn't exist, this is the first run. Note that there's no prior week to diff against, and create the file after this review completes.

## Step 2: Pull This Week's Raw Material

Read three sources in full:

- **`context/decisions-log.md`** — every entry from the last 7 days, plus any older entry still marked Open (decisions don't age out just because the log entry is old).
- **`context/people/*.md`** — for each stakeholder file, read the Interaction log entries from the last 7 days and the full Open items list regardless of date. Note the stated communication style and relationship cadence at the top of each file — you'll need it in Step 5.
- **`context/goals.md`** — the Primary goals, the "How I measure a good week" bullets, and Standing priorities. These are what everything else gets checked against.

## Step 3: Sort the Decisions

Split into two buckets:

- **Decided this week** — status is Closed and the close (or the original log date) falls in the last 7 days. One line each: what was decided, who owned it, why it mattered.
- **Still open** — status is Open, whether logged this week or carried from before. Tag each with how many days it's been open and who owns the next move.

Anything with an unclear status goes in its own short "unclear — needs a status check" list. Don't fold it into either bucket.

## Step 4: Sort the Commitments

Build one combined open-items list across every person file, not per-person silos — a founder needs one queue, not five. For each item: what it is, which stakeholder it's tied to, how long it's been sitting, and whether a due date exists.

Cross-check against Step 3's open decisions — a lot of the time these are the same thing seen from two files. Don't report the relaunch-date decision once from `decisions-log.md` and again from Sam's file as if they're two separate items.

## Step 5: Check the Goals

Go through `context/goals.md`'s "How I measure a good week" bullets one at a time and answer each honestly against what actually happened this week, with evidence from Steps 3-4, not a vibe:

- Did the founder's/exec's top priority move, or did the week just stay busy around it?
- Did anything slip, mine or someone else's, without getting flagged first? (If Step 3/4 turned up something still open past its own due date with no flag raised yet, that's a miss — say so directly.)
- Did anything get written down this week that would otherwise have only lived in a chat thread? (Check whether this week's decisions and interactions actually made it into the log files, or if this review is the first time some of them are being captured.)

Then check the Standing priorities list the same way — plainly, not as a checkbox exercise.

## Step 6: Decide What Needs Escalating

Pull from Steps 3-5 anything that fits:

- **Due today or already past due, and still open.** Always escalate, regardless of how small it looks.
- **Open two review cycles running** (per Step 1's diff). Stuck is a different problem than new, and it needs to be named as stuck.
- **A stakeholder gone quiet relative to their own stated cadence**, especially one tied to an open item.
- **A decision that's about to get made by default** — engineering confidence carrying a date forward, a hire happening because no one investigated the real cause, that kind of thing. This is the "spot the risk before it's a problem" job, not just a status report.

Everything else is just an open item, not an escalation. Don't inflate the list, or the real ones stop standing out.

## Step 7: Write the Review

```
Week ending [date] — weekly review:

Decided this week:
- [Decision] — owner: [who] — [why it mattered]

Still open, carried in:
- [Item] — open [N] days — owner: [who] — [current blocker]

New this week, still open:
- [Item] — owner: [who] — due [date, if any]

Unclear status (needs a check, not a guess):
- [Item] — [what's ambiguous about it]

Goals check:
- [priority/measure] — [on track / drifting] — [the evidence]

Escalate before next week:
- [item] — [why now, not Monday]

Stakeholder pulse:
- [Name] — last contact [date] — [normal for their cadence / gone quiet]

Pattern watch:
- [anything open in 2+ consecutive reviews, named as a repeat, not a fresh item]
```

## Step 8: File the Review

Append a new dated entry to `context/weekly-review-log.md` in this same shape, oldest to newest, never overwriting a past entry. This is what next Friday's Step 1 reads against — skipping this step breaks the whole "still stuck vs just noticed" mechanism.

## Worked Example (Nova Loop)

Running this Friday 2026-06-26 against the seeded example data:

- **Decided this week:** the two-scenario runway model (logged 2026-06-18, closed 2026-06-24) — Elliot wanted burn-vs-hires visibility rather than a single projection, delivered on time.
- **Still open, carried in:** the relaunch-date decision (open since 2026-06-22, owner Priya with Sam's input) — due "end of week," which is today. This is the top escalation, not a routine open item.
- **New this week, still open:** the support-ticket root-cause investigation (opened 2026-06-24) — recommendation was due before Friday board prep, which is also today. Second escalation.
- **Goals check:** "flag drift before it slips" — a pass so far, Priya's hiring instinct got questioned before being actioned. But both open items are due today with no resolution yet logged — if they close by end of day, the week holds; if not, that's a real miss to name next Friday, not soften.
- **Stakeholder pulse:** Sam — last contact 2026-06-22, four days quiet against a weekly-sync cadence, due for a check-in. Priya — last contact 2026-06-24, two days quiet against a daily-relationship cadence, worth a light ping given two items are riding on her today. Elliot — last contact 2026-06-18, fine against a monthly cadence.

## Rules

- Never edit a past decisions-log or interaction-log entry, only read them.
- Never guess a status — unclear goes in its own list, not folded into open or closed.
- An item's age and its due date matter more than how it reads today — check both before deciding whether to escalate.
- Compare against last week's log before calling anything "new."
- Don't escalate everything, only what's actually due, actually stuck, or actually going quiet against its own cadence — a long escalation list is as useless as none.
- If a goal or standing priority hasn't moved in weeks, say so plainly rather than restating it as if it's fresh.

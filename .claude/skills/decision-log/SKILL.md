---
name: decision-log
description: Capture material decisions and the reasoning behind them into context/decisions-log.md, append-only, so nothing gets re-explained or re-litigated from scratch later. Before logging anything new, searches the log for related prior decisions and surfaces them first, so a call never gets silently re-made or reversed without seeing what was decided before. Invoke right after a meeting or briefing produces a real decision (or punts one), when a previously logged decision's status changes, when a past decision is being revisited, or when asked "why did we decide X" and the answer isn't already written down.
user_invocable: true
---

# Decision Log

Write down decisions and the reasoning behind them the moment they're made, so the founder/exec, a new stakeholder, or future me never has to re-litigate something that was already settled. Append-only. Never edit a past entry.

The part that makes this more than a running list: before anything new gets written, the log gets searched for related prior decisions on the same topic, stakeholder, or project, and if one turns up, it gets surfaced to the user before a single word is drafted. The point isn't just to remember decisions, it's to stop the founder from quietly re-deciding, contradicting, or flip-flopping on something already settled, without ever noticing they're doing it.

## Operating Rules (read first)

- **`context/decisions-log.md` is the SSOT for decision history.** If a briefing, update, or answer touches "why did we do X" or "what did we decide about Y," read this file first. Never reconstruct from memory or guess at reasoning that isn't written down.
- **Append-only, newest entry on top.** New entries go directly under the file's intro note, above the most recent existing entry. Never reorder or delete older entries.
- **Never rewrite a past entry's Decision, Why, or Owner text.** Once written, that's the record of what was known and decided at the time. If it turns out to have been wrong, that's information for a new entry, not a correction to the old one.
- **The one exception is the Status line.** Status is allowed to move forward in place (e.g. `Open` -> `Closed, delivered 2026-06-24`) because it's tracking the same decision's lifecycle, not rewriting the reasoning. If the decision itself changes, gets reversed, or the reasoning shifts, that's not a status update, that's a new entry (see Step 5).
- **Never write a fresh entry without searching first.** Step 2 is mandatory, not optional, even when the topic looks brand new. Skipping it is exactly how a decision gets quietly re-made without anyone noticing it contradicts an earlier one.
- **Not every choice belongs here.** This log is for material decisions, not routine execution. See Step 1 for the bar.

## Intended Integrations

None required. This stays local-file-based, reading and writing plain markdown in `context/decisions-log.md` directly, the same ADR-style pattern (append-only, dated, reasoning-first) it already follows. There's no API, sheet, or external system to wire up, and nothing about the challenge step in Step 2 needs one either, it's a search over a file that's already on disk.

The one optional addition: if `context/` were ever opened as an Obsidian vault, this file needs zero conversion, it's already plain markdown with dated headings. Obsidian would just make the trail easier to browse (backlinks from a person file or project note straight to the decision that shaped it), not a new source of truth. Nothing to build for that, it works the moment the folder is opened.

## Step 1: Recognize a Loggable Decision

Log it if any of these are true:

- Someone (the founder/exec, an investor, a stakeholder) made a real call, or explicitly deferred one.
- The reasoning involved a real tradeoff someone could reasonably question later.
- It sets a precedent or approach likely to recur (a policy, a threshold, a "we do it this way now").
- It touches risk, cost, timeline, or reversibility in a way that matters if it goes wrong.
- Someone would need this explained to them again in a month if it weren't written down.

Don't log:

- Routine task execution already tracked elsewhere (a task tracker, meeting notes, a calendar invite).
- One-off trivial choices with no downstream consequence (which template to use, what time to send a reminder).
- Anything that's just your own scratch work in service of a decision, not the decision itself.

When unsure, err toward logging it. A short unnecessary entry costs nothing. A missing entry costs a re-litigated argument three weeks from now.

## Step 2: Challenge — Search for Related Prior Decisions

This is the signature move. Before drafting a single field, search `context/decisions-log.md` for any entry that touches the same topic, stakeholder, or project as the decision in front of you.

1. **Search broadly, not just by title.** Scan headings and the Decision/Why text for the same person, project, or subject, not an exact string match. A decision about "the relaunch date" and one about "launch-week comms" can be the same live thread under different words.
2. **Nothing related turns up:** say so in one line ("checked the log, nothing on this yet") and move to Step 3 as a fresh, unrelated entry.
3. **Something related turns up:** stop and surface it before writing anything, in this shape:

   > "You decided [X] on [date] for [reason]. This new decision looks like it [changes / contradicts / extends] that, confirm you want to log it, and I'll [update the status in place / write it as a Step 5 reversal / log it as a related but distinct fresh entry]."

4. **Wait for the answer.** This is the one place in the skill where drafting ahead of the user is never allowed, even though every other step is fine drafting freely. The whole value of the challenge is that the founder sees their own past reasoning before deciding whether to override it, not after.
5. **Route based on the answer:**
   - Genuinely unrelated once explained -> Step 3, fresh entry (mention the prior entry inline in Why only if it's useful context, don't force a link that isn't real).
   - Closes out or updates an existing open entry without changing its reasoning -> the Status-line exception, not a new entry.
   - Actually reverses, supersedes, or contradicts the prior reasoning -> Step 5, a new entry that references the old one.

## Step 3: Capture the Four Fields

Every entry uses exactly this structure, matching the existing log:

- **Decision:** What was actually decided (or explicitly not decided, and left open as a flagged risk).
- **Why:** The reasoning, including whose instinct or position it overrides or confirms, and what would go wrong with the alternative.
- **Owner:** Who has the final call. If it's not the founder/exec, name whose sign-off it needs before it's real.
- **Status:** `Open` (still live, note what's needed to close it and by when), or `Closed` (note the outcome and date).

If any field is genuinely unknown, write "TBD" and say so out loud to the user rather than filling it with something plausible-sounding. A logged decision with a guessed Owner is worse than no entry.

## Step 4: Write the Entry

Format, inserted at the top of the log (directly under the intro note, above the current top entry):

```
## YYYY-MM-DD — Short decision title

**Decision:** ...
**Why:** ...
**Owner:** ...
**Status:** ...
```

Use today's date. Keep the title short enough to scan in a list of ten entries. Don't touch anything below it.

## Step 5: When a Decision Changes

If a decision gets reversed, superseded, or the reasoning turns out to be wrong (this is usually what Step 2's challenge surfaces), don't edit the old entry. Add a new dated entry that explicitly references the old one:

```
## YYYY-MM-DD — Revisiting: [old decision title]

**Decision:** [what's different now]
**Why:** [what changed, referencing the 2026-06-18 entry above and what new information forced the change]
**Owner:** ...
**Status:** ...
```

This keeps the full trail visible: what we thought, why we thought it, and what we know now. Never delete the earlier version, it's the record of the reasoning at the time, and hiding it makes the founder/exec's own past calls look worse than they were.

## Step 6: Retrieval (using the log, not just writing it)

Before answering anything that references past decisions, precedent, or "why did we do it that way":

1. Read `context/decisions-log.md` in full, it should stay short enough to scan.
2. Cite the specific dated entry in your answer, don't paraphrase from memory.
3. If the log doesn't cover it, say so explicitly rather than inventing a plausible-sounding rationale.

Step 2 is this same discipline, just made mandatory and front-loaded, specifically for the moment right before something new gets written.

## Worked Examples

**Challenge search catching a live re-decision (the signature move):**

Someone wants to revisit the relaunch date on 2026-06-28. Before drafting anything, search the log for "relaunch." Two related entries turn up: `2026-06-22 — Relaunch date under review` (flagged as an open risk, decision needed by end of week) and `2026-06-24 — Investigate support-ticket spike before deciding on hiring` (holding on hiring pending root-cause research, which touches the same relaunch thread). Surface both before writing anything:

```
On 2026-06-22 you flagged the relaunch date as a live risk because support/CS
wasn't looped into launch-week comms, decision needed by end of week. On
2026-06-24 you held off on hiring because the ticket spike might be a
comms problem, not a capacity one, pending root-cause research. Is this new
call closing one of those, or is it a different decision about the relaunch?
Confirm before I log anything.
```

Only after the user answers does this become a fresh entry, an in-place status close on one of the two, or a Step 5 reversal. That's the actual value of the log: it stops a decision from getting re-made in the dark, three weeks after the first one was already settled.

**Logging a fresh entry (challenge search came up empty):**

Checked the log for "pricing page" and "Priya," nothing related exists yet. Clear to log fresh:

```
## 2026-07-01 — Delay the pricing page change until Q3 data is in

**Decision:** Hold the pricing page update Priya wanted to ship this week.
**Why:** The A/B test on the current page hasn't reached significance yet. Shipping now would mean deciding blind on a change that's expensive to reverse (SEO + existing customer confusion).
**Owner:** Priya (final call), me (flagged the risk before she committed)
**Status:** Open, revisit once the test hits significance, expected mid-July.
```

**Logging a status update on an existing entry (in place, per the Status exception):**

Find the entry, e.g. `## 2026-06-18 — Board wants runway under two scenarios`, and update only the Status line:

```
**Status:** Closed, delivered 2026-06-24.
```

Leave Decision, Why, and Owner exactly as written.

**Logging a reversal (new entry, not an edit):**

```
## 2026-07-05 — Revisiting: relaunch date

**Decision:** Relaunch pushed two weeks, to accommodate the support-comms fix from the 2026-06-24 entry.
**Why:** The ticket-spike root cause turned out to be launch-comms, not capacity. Fixing comms needs the extra two weeks; shipping on the original date would repeat the same gap.
**Owner:** Priya
**Status:** Closed, new date confirmed.
```

## Rules

- Never write a fresh entry without running the Step 2 challenge search first, even when the topic looks new. This is the check that makes the log worth having.
- If the challenge search surfaces a related entry, show it to the user and wait for confirmation before writing anything. Never resolve the ambiguity silently on their behalf.
- Append-only. Never edit a past entry's Decision, Why, or Owner.
- Status lines are the sole in-place exception, for lifecycle tracking only.
- Every entry needs a real Owner, or an explicit "TBD" flagged to the user, never a guess.
- When retrieving history, cite the entry. Don't reconstruct reasoning from memory.
- If you're not sure whether something is material enough to log, log it anyway, the cost of a short unneeded entry is near zero.

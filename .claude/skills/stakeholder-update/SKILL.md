---
name: stakeholder-update
description: Turns raw notes, decisions, and metrics into an update shaped for who's reading it. Board decks use Sequoia's 3-hour board-meeting framework with SCQA-structured deep dives on the topics that need real discussion. Investor emails use the 5-15 format (readable in 5 minutes, written in 15). Internal leadership updates stay narrative-first. Reads context/people/ for that stakeholder's stated preferences and context/decisions-log.md + context/goals.md for source material. Run before a board pack, an investor update email, or a leadership readout.
user_invocable: true
---

# Stakeholder Update

Turn what actually happened into an update shaped for the person reading it, not a generic status report. A board deck, an investor email, and an internal leadership update are three different documents, even when they cover the same two weeks and the same facts. The first question to answer is who this is for and what shape that person actually needs, and that comes before "what happened."

## Operating Rules (read first)

- **Shape follows audience, not habit.** Name the shape out loud before drafting: Sequoia's 3-hour board-meeting framework with SCQA deep dives for a board deck, the 5-15 format for an investor email, narrative-first for internal leadership. Don't default to whichever one I find easiest to write.
- **A stakeholder's stated preference beats the bucket default.** Their file in `context/people/` overrides whatever the generic shape says, every time.
- **Never smooth a risk to make the update read better.** If a stakeholder wants risks surfaced early, a clean paragraph that quietly omits the risk is a failure, not a good draft.
- **Source only from what's logged.** Pull from `context/decisions-log.md`, `context/goals.md`, and the stakeholder's file. Don't invent a number, date, or outcome that isn't there. Missing data gets flagged as `[NEED: ...]`, not estimated.
- **Draft, don't send.** This produces a draft for the founder/exec to review and approve before it reaches the board, an investor, or leadership. Per the root `CLAUDE.md` operating principle on gating at the edge, nothing external goes out without sign-off.
- **Every line traces back to a source.** If I'm leaning on a decisions-log entry that's been open a while with no movement, say so instead of restating it as fresh news.

## Step 1: Identify the audience and read their file

Check `context/role-target.md` for who I'm working for, then figure out who this specific update is for (named directly, or implied, as when "prep the board update" means the board / lead investor). If it's a board or investor audience, also pin down which document this actually is: a **board deck** for a live board meeting, or an **investor email** written for someone reading it async on their own time. Those are different jobs even inside the same bucket, so ask if it isn't obvious.

Read `context/people/<name>.md` in full and pull out:

- Stated communication style (numbers-first vs. narrative, format preference)
- What they've said they care about (specific metrics, specific concerns)
- What they've explicitly said they dislike (surprises, vanity metrics, being managed)
- Open items already attributed to them (things they're waiting on)

If there's no file for the named audience, stop and ask who this is functionally closest to. Don't guess a generic tone for someone I have no read on.

## Step 2: Pull the source material

Read `context/decisions-log.md` in full and pull every entry relevant to the update window (since the last update, or a given date range). Read `context/goals.md` so progress gets framed against stated priorities, not just listed chronologically.

Cross-reference the log against the stakeholder's open items. If something they asked for has since been delivered, name that closed loop explicitly instead of leaving it implied.

## Step 3: Pick the shape

Three shapes, named explicitly so this doesn't turn into a generic "update" template. Board/investor splits into two, depending on what's actually being produced.

**Board deck — Sequoia's 3-hour board-meeting framework:**
Treat the board's time as scarce. Materials get sent ahead so the room debates instead of reads. The meeting itself is structured, and the deck should be built to match it:
1. Pre-read package, sent days ahead. No live board time gets spent presenting numbers people could've read.
2. A short context-setting block (roughly the meeting's first 30 minutes): headline metrics, what changed, stated fast.
3. The bulk of the meeting (the remaining two-plus hours) goes to 2-3 real discussion topics, the things that actually need the board's judgment, not a status readout. Each of those topics gets its own **SCQA** brief: **Situation** (stable context before anything changed), **Complication** (what changed that makes this worth board time), **Question** (the question the complication forces), **Answer** (the recommendation or the decision being asked of the board).
4. Remaining time: other business, exec session if needed.

Don't SCQA a topic that's just a status update. That's what the pre-read metrics block is for. SCQA is reserved for the topics that need real discussion.

**Investor email — 5-15 format:**
Written for a reader on their own time, not in a room. The standard: readable in 5 minutes, writable in 15. That constraint forces prioritization instead of a comprehensive report nobody finishes.
1. Headline metrics: the handful this specific investor tracks, not everything measurable
2. What changed since the last update, in a few lines, not paragraphs
3. Risks and open items, named plainly, not buried
4. What's actually being asked of them, if anything, or "nothing needed this cycle"

**Internal leadership — narrative-first:**
1. What we're solving for this cycle (tie to a `goals.md` priority)
2. What happened (can read as a short story: what we tried, what worked, what didn't)
3. What's next
4. What's blocking, and what would unblock it

Across all three: a stakeholder's stated preference overrides their bucket's default. Elliot Marsh is board/investor, but his file says numbers first, narrative second, and that instruction wins over whichever generic template his bucket implies.

## Step 4: Draft

Write the update in the chosen shape, sourced only from Steps 1 and 2. Every claim ties to a specific decisions-log entry or a real number from the source material. No number on hand means `[NEED: what's missing]` in the draft, not a filled-in guess. Match length and reading level to what the stakeholder's file implies. A founder who wants short briefs gets a short brief, not a memo.

## Step 5: Self-check against their stated preferences

Before presenting anything, reread the "cares about" and "dislikes" lines from Step 1 and check the draft against each one by name. This is the step that catches a buried risk or a vanity metric leading the doc before the founder ever sees it, so do it explicitly and don't assume the draft is fine because it "reads well."

## Step 6: Present for approval

Hand over the draft plus the Step 5 check to the founder/exec. Never send or publish directly. This is always a draft for their review first. Flag anything sourced from a stale or still-open decisions-log entry so they can decide if it's actually ready to go out.

## Intended Integrations

This repo runs on flat files (`context/decisions-log.md`, `context/people/`) so the demo works with zero external accounts. In a live deployment:

- **Board decks** — draft the SCQA-structured content here, then push it into a real deck via the **Google Slides MCP** (or hand off as a **Google Doc** for a text-first board memo), so the founder edits and shares it in the format the board actually receives, not a Markdown file only this repo can open.
- **Investor emails** — compose the 5-15 draft here, paste into a **Google Doc** for a shared review pass if the founder wants a second set of eyes, then send once approved through whatever channel that investor relationship actually uses.
- **Internal leadership updates** — sync the narrative-first draft into **Notion**, since that's usually where internal readouts already live and get commented on, rather than staying trapped in a Markdown file only the founder opens.

The skill still drafts and self-checks locally first, every time. Approval happens before anything reaches Slides, Docs, or Notion. The integration point is where an already-approved draft ships, not where it gets written.

## Worked Example: Investor Update for Elliot Marsh (Nova Loop) — 5-15 format

Source: `context/people/elliot-marsh-investor.md` says numbers first, narrative second, dislikes surprises more than bad news, cares about burn rate and NRR over vanity metrics, wants risks surfaced early. This is a monthly async update, not a live board meeting, so it's the 5-15 shape: `context/decisions-log.md` has three relevant entries — the two-scenario runway model he asked for on 2026-06-18 (delivered 2026-06-24), the support-ticket spike investigation (open), and the relaunch-date review (open).

```
Nova Loop — Investor Update — Elliot Marsh

Numbers
- Runway (two-scenario model, delivered this cycle): [see attached scenario doc —
  actual burn/hire-case figures live there, not restated here from memory]
- Burn rate: [NEED: current monthly burn — not in decisions-log.md, confirm with
  Priya's team before this goes out]
- NRR: [NEED: not logged this cycle — flag as missing, don't estimate]

What changed since last update
- Delivered the two-scenario runway model (current burn vs. planned hires) requested
  2026-06-18. Closed 2026-06-24.

Risks — surfaced now, not smoothed over
- Support ticket volume up 30% since the relaunch. Root cause not yet confirmed.
  Could be a launch-comms gap, not a capacity problem. Holding off on additional
  support hires until that's clear. Recommendation due before Friday's board prep.
- Relaunch date is under review. Support/CS wasn't looped into launch-week comms
  planning. Not resolved yet, Priya deciding by end of week.

Asking of you
- Nothing needs a decision from you this cycle. Both items above are flagged now,
  before the call, not after.
```

**Self-check against Elliot's file:** numbers first, narrative second, yes, the numbers block leads. Dislikes surprises more than bad news, and both open risks are named here, not held until resolved and sprung on him live. Cares about burn/NRR over vanity metrics, so only burn/NRR-relevant lines made the headline block. Burn rate and NRR actuals aren't logged anywhere this cycle, flagged as `[NEED]` rather than filled in with a plausible-looking number.

**If this were a board-meeting deck instead** of an async email, the ticket-spike risk above would become its own SCQA discussion brief rather than a bullet:
- **Situation:** Support ticket volume was stable through the relaunch window.
- **Complication:** Volume is up 30% since relaunch, and root cause isn't confirmed. Could be a comms gap, could be a real capacity problem.
- **Question:** Do we approve support hires now, or wait for the root-cause read?
- **Answer:** Hold on hires until the root-cause finding lands, due before this meeting's prep cutoff. [NEED: confirm the finding is actually in hand before the meeting, not still pending.]

## Rules

- Never fabricate a number, date, or outcome that isn't in `decisions-log.md` or the stakeholder's file.
- Never let "make it read better" override a stated preference for surfacing risk.
- Shape follows the stakeholder's stated preference and the actual document being produced, not whichever one is easier to fill in.
- SCQA is for topics that need real board discussion, not for status updates that belong in the pre-read metrics block.
- This is always a draft. It doesn't go out, or reach Slides/Docs/Notion, without the founder/exec's review and approval.
- No file for the named audience means stop and ask, not guess their tone.
- A decisions-log entry that's been open a long time with no movement gets flagged as stale, not quietly restated as fresh.

---
name: research-brief
description: Runs competitive intelligence in two modes. Weekly Digest turns scattered competitor updates into a threat-scored brief tied to context/goals.md (diff-only after the first run, hard 300-word cap). Ad-Hoc reacts the moment the founder flags one event, a launch, a pricing change, a hire, with the same threat rating and forced owner-plus-action discipline, scoped to that single event. Tracks five signal categories, product/UX, pricing, hiring, partnerships, messaging. Every High-threat signal gets a named owner and a concrete recommended action, "just monitor" is not a valid response. Run this on the weekly cadence for the scheduled digest, or immediately whenever the founder pings a competitor update and asks "what does this mean for us."
user_invocable: true
---

# Research Brief

Competitive intelligence for a founder who doesn't have time to read the market himself. Two modes, same discipline: a scheduled weekly sweep across every tracked competitor, and an immediate reaction to a single event the founder just noticed. Neither mode is an activity log. A competitor doing something is not news by itself, it only earns a line in the brief once it's been judged against what this founder is actually trying to protect right now.

This borrows its two-mode shape from the `competitive-intelligence-monitor` and `competitor-signal-tracker` skills in `mohitagw15856/pm-claude-skills`, combined into one skill because they're the same underlying job at two different cadences, not two different jobs. Neither source skill ships a real research connector, they're framework logic. This version wires that logic to WebSearch/WebFetch and to this repo's own `context/goals.md` and `context/people/`, see Intended Integrations below.

## Operating Rules (read first)

- **Name the mode out loud before starting.** Weekly Digest (scheduled, every tracked competitor) or Ad-Hoc (one event, right now). Don't blend them, an ad-hoc ping doesn't wait for the weekly cycle and the weekly digest doesn't skip a competitor because an ad-hoc brief already touched them once.
- **`context/goals.md` is the filter, not a footnote.** A signal only earns space in the brief once it's judged against a Standing priority or a specific stakeholder's stated concern (`context/people/`). "Competitor X shipped Y" is not a finding until it says what Y means for what Priya-equivalent is optimizing for this week.
- **Five categories, nothing else.** Product/UX, pricing, hiring, partnerships, messaging. If a piece of news doesn't fit one of those, it's not a tracked signal, it's trivia.
- **Every High-threat signal needs a named owner and a concrete action before it's done.** "Just monitor" is explicitly disallowed as the response to a High. Medium or Low can land on "monitor" if that's genuinely the right call, but say so and name what would upgrade it.
- **Diff-mode after the first run, weekly only.** The first-ever Weekly Digest is a full baseline sweep. Every run after only reports what's new or changed since the last logged digest, not the whole landscape again.
- **Hard 300-word cap on the Weekly Digest.** Ad-Hoc has no cap, it's one event and the founder is reading it in real time.
- **`context/competitive-intel-log.md` is the append-only record for both modes, newest entry on top.** It doesn't exist yet on a fresh engagement, that's expected. Create it with the first entry rather than skipping the skill or asking permission to start it.
- **No competitor list is seeded in this repo.** If `context/competitors.md` doesn't exist or is empty, ask the user which competitors to track before running a Weekly Digest. Ad-Hoc mode doesn't need the list, the founder is naming the competitor and event directly.
- **Draft only, gated at the edge, same as everything else in this repo.** This produces a brief for the founder/exec to read and act on. It never sends, posts, or forwards anything externally on its own.

## Step 0: Name the mode

A scheduled sweep, or asked for as "this week's competitor update," is Weekly Digest. The founder relaying a single thing ("just saw Ridgeline launch X," "they just dropped pricing," "they hired a VP of Y") is Ad-Hoc. If it's genuinely ambiguous, ask which one before researching anything.

## Weekly Digest Mode

**Step 1: Confirm the tracked list.** Read `context/competitors.md`. Missing or empty means stop and ask who to track, don't invent a plausible-looking competitor set.

**Step 2: Research each competitor across the five categories.** WebSearch/WebFetch each one for product/UX changes, pricing pages, job listings (hiring signal), announced partnerships, and any shift in how they're messaging themselves (site copy, launch posts, positioning).

**Step 3: Baseline vs. diff.** Read `context/competitive-intel-log.md`. Empty file means this is the baseline sweep, report everything found. A prior entry exists means report only what's new or changed since that entry, not a re-statement of the whole landscape.

**Step 4: Score every candidate signal.** Rate High/Medium/Low against the rubric below, and name the specific `goals.md` priority or `context/people/` concern it ties to. A signal with no tie to either doesn't make the cut, it's industry noise, not this founder's business.

**Step 5: Force owner and action on every High.** Per the Operating Rules, no High reaches the draft without both fields filled in for real.

**Step 6: Write to the cap, append to the log.** Order by threat descending. If the draft runs over 300 words, cut Lows first, then Mediums, never trim a High to make room. Append the full findings (including anything cut for space) to `context/competitive-intel-log.md` so the next diff run has the real history, then present the capped digest to the user. Nothing auto-sends.

## Ad-Hoc Mode

**Step 1: Take the event as given, then verify what's missing.** Trust the founder's report of what happened, but if a material fact is missing (exact price, launch date, who exactly was hired), do a quick WebSearch/WebFetch pass to confirm before rating it. Don't rate on a guess.

**Step 2: Categorize.** Which of the five signal categories this event actually is. One event can touch more than one, say so if it does.

**Step 3: Score threat, tie to goals.md.** Same rubric and same requirement as the weekly mode, no shortcut for being fast.

**Step 4: Owner and action forced.** Same discipline as Step 5 above. A High rated in five minutes still needs a real owner and a real next step, not "keep an eye on it."

**Step 5: Present immediately, log it now.** Don't wait for the weekly cycle. Append the dated entry to `context/competitive-intel-log.md` right away, so the next Weekly Digest diff sees it as already-covered and doesn't re-report it as new.

## Threat Level Rubric (shared by both modes)

- **High** — directly threatens a stated `goals.md` priority, an open item in `context/people/`, or the thing the founder/exec is actively spending attention on this week (a live deal, an in-flight relaunch, a metric already flagged as drifting).
- **Medium** — relevant to the competitive space and worth knowing, doesn't force a near-term response. Log it, don't force an owner unless one naturally applies.
- **Low** — background noise. Doesn't move anything currently being optimized for. Still logged, so the diff history stays complete, but only makes the capped weekly text if there's room left after every High and Medium.

## Intended Integrations

- **WebSearch and WebFetch (native, no MCP setup needed)** are the primary and only integration this skill strictly requires. Every research step above assumes them directly, this skill works fully with zero external accounts.
- **Optional, for the Weekly Digest's ongoing monitoring:** a news RSS feed per tracked competitor (most company blogs and press pages expose one) or a Google Alerts-style saved search, feeding candidate signals into Step 2 instead of a cold full-web search every single week. Not required, WebSearch alone covers the job, but it cuts search cost and noise once a real engagement has three or more competitors tracked for months at a time.

## Worked Example: Ad-Hoc Mode, Nova Loop

Setup, from the seeded context: Nova Loop just went through a product relaunch, and Priya's interaction log shows she wants the churn number "reframed against cohort, not blended average," which points at a cohort-level retention view as a centerpiece of that relaunch. `context/decisions-log.md` (2026-06-24, 2026-06-22) shows the relaunch is already a live risk area, support ticket spike, comms gap, date under review.

Priya sends a two-line message: *"Just saw Ridgeline shipped a cohort retention dashboard, looks close to what we just launched. Undercutting us $10/seat too. Thoughts?"*

Running Ad-Hoc mode:

**Step 1 (verify):** WebFetch Ridgeline's pricing and changelog pages to confirm the price gap and check whether the feature is actually cohort-level or just a labeled churn chart. Confirmed: real cohort breakdown, live as of two days ago, $10/seat under Nova Loop's tier.

**Step 2 (categorize):** Two categories at once, product/UX (feature parity on the exact thing Nova Loop just relaunched around) and pricing (direct undercut).

**Step 3 (score + tie to goals.md):** **High.** Ties directly to Standing priority 3 ("keep the relationship map current," Sam and Priya are both mid-conversation about relaunch messaging) and to the still-open 2026-06-22 decisions-log entry on the relaunch date. This isn't generic market news, it lands on the exact feature Nova Loop is currently staking the relaunch narrative on, during a week that's already flagged as a live risk.

**Step 4 (owner + action, forced):** Not "keep watching Ridgeline." Recommended action: draft a one-page differentiation note before Priya's next investor touchpoint (real-time cohort updates built into the existing workflow vs. Ridgeline's standalone dashboard, per what's actually on their changelog), and flag to Sam whether the product roadmap should pull forward the next cohort-view improvement rather than let a $10/seat gap sit unanswered. **Owner:** me (draft the note and the roadmap flag), Priya (final call on whether it goes to Sam this week or next).

**Step 5 (present + log):** Hand Priya the one-line rating plus the draft note for review, then append to `context/competitive-intel-log.md`:

```
## 2026-07-02 — Ad-Hoc — Ridgeline ships cohort retention dashboard, $10/seat under our tier

**Category:** Product/UX, Pricing
**Threat:** High
**Tied to:** Standing priority 3 (context/goals.md); open relaunch-date decision (decisions-log.md, 2026-06-22)
**Recommended action:** Draft differentiation one-pager before next investor touchpoint; flag to Sam whether to pull forward the next cohort-view roadmap item.
**Owner:** Me (draft), Priya (final call on timing/audience)
**Status:** Open, draft due before Priya's next investor call.
```

That entry is what keeps this from becoming an activity log. It doesn't say "Ridgeline launched a feature," it says what that feature means for the exact thing Nova Loop is mid-relaunch on, and it leaves a specific action with a specific owner, not a note to keep watching.

## Weekly Digest Format Sample

Not a second full walkthrough, just the shape the capped, diffed digest actually takes, reusing the same Ridgeline signal as if it had first surfaced on the weekly cycle instead of ad-hoc:

```
Nova Loop — Competitive Digest — Week of 2026-06-29
(Diff since 2026-06-22 digest, 2 new/changed signals)

HIGH — Ridgeline: cohort retention dashboard, $10/seat under our tier
Product/UX + Pricing. Direct hit on the relaunch's centerpiece feature, during
an already-flagged relaunch-risk week (decisions-log 2026-06-22).
Action: differentiation one-pager before next investor touch. Owner: me
(draft), Priya (timing).

MEDIUM — [Competitor]: two support-eng roles posted, remote
Hiring. Possible capacity build-out, not urgent. No owner assigned yet,
revisit if a launch follows within 60 days.

No changed signals this week for [Competitor 3], [Competitor 4].
```

That's the whole point of the cap and the diff: a two-minute read that only tells the founder what actually moved since last week, not a re-run of the full competitive landscape every seven days.

## Rules

- Five signal categories only, product/UX, pricing, hiring, partnerships, messaging. Nothing else counts as a tracked signal.
- Every High gets a named owner and a concrete action. Never "just monitor" a High.
- Weekly Digest: first run is a full baseline, every run after is diff-only against the last logged entry, and the 300-word cap is enforced by cutting Lows first, never by trimming a High.
- Ad-Hoc entries get logged immediately, not held for the next weekly cycle, so the diff mode never double-reports them as new.
- No competitor list means ask, never invent a plausible-looking one.
- A signal with no real tie to `context/goals.md` or `context/people/` doesn't make either brief, it's market noise, not this founder's business.
- If a fact is unconfirmed (rumored pricing, an unannounced hire), say so plainly in the brief rather than rating it with invented certainty.
- Nothing produced here goes external without the founder/exec's review and approval first, same as every other skill in this repo.

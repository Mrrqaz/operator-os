---
name: okr-kpi-tracker
description: Sets quarterly objectives and key results, breaks each key result down into the component metrics that actually drive it, then tracks that whole tree against context/kpi-dashboard.md and flags drift the moment it's noticed instead of waiting for the next scheduled review. Every flag gets root-caused down to the specific metric that moved, not just "the top number is red." Run this to set or revisit OKRs, whenever a metric moves, before any board/investor update, or whenever asked "how are we tracking" / "check the KPIs" / "anything drifting."
user_invocable: true
---

# OKR / KPI Tracker

This is the company-wide goal-setting and metrics half of the job: turn a quarter's priorities into real objectives and key results, decompose each key result into the handful of metrics that actually move it, then keep that whole structure honest so nothing drifts unnoticed. A markdown table is the dashboard. This is a Claude Code repo, not a BI product, don't build more than the job needs.

The three-stage shape here (objective/key-result setting, then metric decomposition, then measurement and drift) follows the pattern used by the OKR Builder, Metric Tree Builder, and Metrics Framework skills in `mohitagw15856/pm-claude-skills` (1.1k stars, eval-scored ~4.8/5 average across 208 evaluated skills, the strongest credibility signal found researching this space). None of those are tool-connected, they're pure framework logic. This skill borrows the structure and wires it to a real data layer and a real drift/recommendation loop, see Intended Integrations below.

## Operating Rules (read first)

- **Key results before metrics, metrics before dashboard rows.** Don't add a tracked number until it's clearly a component of a key result. If you can't say which KR a metric serves, it doesn't get a row yet.
- **Key results are measurable, not aspirational.** "Improve support" is not a key result. "Support ticket backlog under 24hr median resolution" is. Each objective gets 2-4 key results, each key result is a number with a target.
- **`context/kpi-dashboard.md` is the source of truth for tracked structure.** This repo does not seed one, there's nothing universal to track until you're inside a real engagement. The first time this skill runs for real, check if it exists. If not, say so plainly and build the OKR tree with the user, don't assume a dashboard exists and invent readings for it.
- **Every key result maps to something in `context/goals.md`.** Specifically to the Standing priorities or the "how I measure a good week" bullets. If someone proposes an objective that doesn't obviously serve one of those, ask why before adding it. Objectives without a reason to exist are how OKR docs rot into theater.
- **Flag drift the moment you see it, at whatever level of the tree it shows up.** `context/goals.md` says the job is measured partly by "did any commitment slip without me flagging it first." A key result is a commitment with a number attached, and a component metric drifting is the early warning for the key result drifting later. Don't sit on it until a scheduled review surfaces it.
- **Never quietly move the goalposts.** If a target or threshold looks wrong when you're reviewing it, say so and ask. Don't adjust it yourself to make a red status read as green. If you did adjust a target, that's a decision, log it.
- **A flag isn't done until it has a recommendation, rooted at the right node in the tree.** "Support tickets are up 30%" is an observation about the top metric. "Support tickets are up 30%, and the tree shows it's almost entirely relaunch-confusion tickets rather than bug reports or capacity overflow, recommend a comms fix before touching headcount" is the job.
- **History is append-only.** Every refresh adds a new dated row per metric. Don't overwrite the last reading, the trend line is the point, not just the current value.
- **Speak the stakeholder's language.** If a key result is something a specific person cares about (an investor who wants burn and NRR, not top-line growth, see `context/people/`), tag it so the recommendation is framed the way they'd actually read it, not generic.
- **Gated at the edge, same as everything else.** This skill drafts objectives, key results, the metric tree, and the flag with its recommendation. It does not message the founder's stakeholders, update a board deck, or decide the response. Draft, then hand off per the CLAUDE.md operating principles.

## What counts as drift

Drift can show up at the key-result level or several branches down in a metric tree, and the second case matters more because it's caught earlier:

1. **A reading (KR or component metric) crosses from green into amber or red.**
2. **The trend line will cross a threshold before the next scheduled check-in, even though it hasn't crossed yet.** Waiting for the actual breach before saying anything is exactly the "flag it at the scheduled review" failure this skill replaces. If three readings in a row are moving toward red, that's already worth a flag, whether it's the headline KR or a metric two levels under it.

## Steps

### 1. Set or confirm objectives and key results

Check `context/kpi-dashboard.md` for existing OKRs. If none exist, build them with the user from `context/goals.md`'s standing priorities and whatever the founder/exec has stated caring about (check `context/people/`). Each objective gets 2-4 key results, each key result is a specific number with a target and a timeframe, not a vague direction.

### 2. Build the metric tree for each key result

For every key result, decompose it into the 2-5 component metrics that actually drive it. Example shape: KR = "grow net revenue," tree = new-customer revenue + expansion revenue - churned revenue. The tree is what lets step 6 answer "why did this move" instead of just "this moved." Keep it shallow, one level of decomposition is usually enough, don't build a metric taxonomy nobody will read.

### 3. Refresh readings

Pull the latest value for each metric in the tree, ideally from the connected Google Sheet (see Intended Integrations), or from wherever it actually lives if the sheet isn't wired up yet (a report, a stated update from the founder). Don't fabricate a number if the source isn't available, mark the row "no reading, source unavailable" instead of guessing.

### 4. Compare against band and trend

For each KR and each component metric: where does the new reading sit against its green/amber/red band, and where do the last 2-3 readings point. Flag using the drift definition above, not just a bare threshold check, and check every level of the tree, not only the top-line KR.

### 5. Root-cause pass, walking down the tree

For every KR that's drifting, walk its metric tree to find which component actually moved. That's the difference between "revenue is soft" and "revenue is soft because expansion revenue stalled, new-customer revenue is on plan." Also check what changed around the same time (a launch, a policy change, a seasonal pattern) and whether this is the problem it looks like or a different problem wearing its clothes.

### 6. Draft the flag and recommendation

One paragraph per drifting node: what moved, since when, which branch of the tree it traces to, likely cause (labeled as inference, not fact, if you're not certain), and a specific recommended action or a specific question to resolve before acting. If the cause is genuinely unclear, the recommendation can be "investigate X before deciding," that's still a real recommendation.

### 7. Update the dashboard, the sheet, and the decisions log if needed

Append the new reading row to `context/kpi-dashboard.md`, and write back to the Google Sheet if that's the connected source of truth for targets/actuals. If the drift is significant enough that it changes what someone should do next (hire, don't hire, change the date), that's a decision in progress, note it and point to logging it properly in `context/decisions-log.md` once the founder/exec has actually decided (append-only there too, never edit a past entry).

### 8. Present to the user, don't auto-send

Show the OKR tree, the flags, and the recommendations in-session. Nothing here gets forwarded to a stakeholder, a board deck, or an update without explicit sign-off.

## Intended Integrations

**Google Sheets, via a Composio-style connector, is the primary named integration.** The OKR Builder / Metric Tree Builder / Metrics Framework patterns this skill is built on are pure framework logic, none of the source skills are tool-connected. That's fine for defining the tree, but a KPI tracker is useless without a real place where targets and actuals live and get updated by people who aren't running Claude Code. A shared Google Sheet is that realistic data layer: the founder/exec's team edits targets and drops in actuals there, and this skill reads current values from it in Step 3 and writes new readings back to it in Step 7, alongside the append-only log in `context/kpi-dashboard.md`. Treat the Sheet as the live source of truth for numbers and `context/kpi-dashboard.md` as the durable, versioned history and narrative layer next to it. Wire the connector before relying on Step 3's sheet path, until then this skill falls back to manually stated readings.

## Worked example (Nova Loop scenario)

Using the fictional example already seeded in `context/decisions-log.md` and `context/people/priya-nakamura-ceo.md`: support ticket volume spiked 30% right after the product relaunch. Priya's first instinct was to hire. Here's what the OKR tree and dashboard would have caught before it needed a board mention, not after.

Objective: **Keep post-relaunch support healthy without over-hiring.** Key result: support ticket backlog stays under 24hr median resolution. Metric tree under that KR: relaunch-confusion tickets, genuine bug reports, and support capacity (agent-hours available). Splitting the tree is what turns "tickets are up 30%" into an actual root cause.

| KR / metric | Band (green/amber/red) | Last reading | Trend | Status | Linked goal | Notes |
|---|---|---|---|---|---|---|
| Support backlog (median resolution) | <24hr / 24-48hr / >48hr | 30hr | 3 readings rising | AMBER | Standing priority 2: don't let a stall go silent | KR level, tree below explains why |
| — Relaunch-confusion tickets (share of volume) | <15% / 15-40% / >40% | 62% of the spike | rising fast | RED | Traces the KR drift to its source | Coincides exactly with relaunch date |
| — Genuine bug reports (share of volume) | <15% / 15-30% / >30% | 18% of the spike | flat | GREEN | Rules out a quality regression | Not the driver |
| — Support capacity (agent-hours available) | on plan / -10% / -20%+ | on plan | flat | GREEN | Rules out a staffing shortfall | Capacity isn't the bottleneck |
| Burn rate (monthly) | within plan / 10% over / 20%+ over | on plan | flat | GREEN | Elliot-relevant: cares about burn over vanity growth | No action needed, tag kept live because Elliot asks every board call |
| Net revenue retention | >100% / 95-100% / <95% | 98% | slight downward | AMBER | Elliot-relevant | Watch, second consecutive amber would be a flag |

The flag that would have gone out on 2026-06-24, before Priya defaulted to hiring: *"Support backlog is up to 30hr median, three readings in a row trending up. The tree traces it: 62% of the spike is relaunch-confusion tickets, genuine bug reports are flat at 18%, and support capacity hasn't dropped. This is a comms problem, not a capacity problem, hiring would patch the wrong branch and be expensive to reverse. Recommend a same-day FAQ/in-app note addressing the relaunch change before Friday's board prep, then re-check the tree in 48 hours."* That's the decision later logged in `context/decisions-log.md` under 2026-06-24, and it's a stronger flag than "tickets are up 30%" precisely because the tree pointed at the branch that moved.

The burn rate and NRR rows aren't drifting today, but they stay on the dashboard because Elliot has said he cares about them over vanity growth (`context/people/elliot-marsh-investor.md`). An OKR/KPI dashboard for this job isn't just "what's broken," it's also "what this specific stakeholder will ask about regardless," so the founder never gets caught flat in a board call.

## Rules

- No `context/kpi-dashboard.md` yet on a fresh engagement, that's expected. Say so and build the OKR tree with the user, don't silently skip the skill or invent one.
- Every key result ties back to a standing priority in `context/goals.md`. No orphan objectives.
- Every tracked metric ties back to a key result it decomposes. No orphan metrics.
- Every drift flag names the specific tree node it traces to and ships with a recommendation, not just an observation.
- Append, never overwrite, both the dashboard history and the decisions log.
- Don't adjust a target to avoid a red status. Flag the target as wrong instead, if that's genuinely the issue.
- Treat the Google Sheet connector as the live numbers source once wired up, and the dashboard file as the durable history. If the connector isn't set up yet, say so and fall back to manually stated readings rather than pretending the sheet is live.
- Nothing produced here goes to a stakeholder without the founder/exec's sign-off first.
- If the root cause is genuinely unclear even after walking the tree, say that directly rather than guessing, and recommend the specific next step to find out.

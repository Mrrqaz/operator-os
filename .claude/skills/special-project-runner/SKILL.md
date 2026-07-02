---
name: special-project-runner
description: Scope, track, and close out a cross-functional special project end to end, from first intake through done. Builds a RACI matrix per workstream, a Red/Amber/Green health rollup, and a Confirmed/Assumed/Not-yet handoff checklist that catches the gap where one team assumes another is looped in but isn't. Invoke when starting a new special project, when checking status on one already running, or before any milestone that touches more than one team.
user_invocable: true
---

# Special Project Runner

Special projects are the part of this job that doesn't run itself. Nobody's chasing me for a status update, and nothing forces a cross-functional gap into the open before it costs a date. This skill is how I scope a project so it's actually trackable, how I plan who's supposed to be involved, and how I keep checking that a gap between teams gets caught by me, not by the customer.

## Operating Rules (read first)

- **One tracker per project, at `context/projects/[project-name].md`.** This repo doesn't seed that folder or any example file in it, I create the first one the first time I scope a real project. Don't assume a tracker exists just because a project is being discussed, check first.
- **Read `context/people/*.md` and `context/decisions-log.md` before scoping anything cross-functional.** Stakeholder files carry the standing context and interaction log I need to know who's already raised a concern. Decisions-log carries anything already decided about this project that the tracker needs to inherit, not re-litigate.
- **RACI and the handoff checklist are not the same question, keep them both.** RACI says who SHOULD be Responsible, Accountable, Consulted, or Informed on a workstream, it's a plan, set once at intake. The handoff checklist says whether that person ACTUALLY knows, it's a live status, updated at every check-in. A team can be correctly marked Consulted in the RACI and still never have been asked anything. I looked at collapsing these into one structure and decided against it, RACI hides the exact failure this skill exists to catch if it's treated as proof someone was told.
- **The handoff checklist is the actual point of this skill.** A project plan with milestones and owners looks complete and still hides the failure mode: Team A assumes Team B knows, Team B finds out at the worst moment. Every milestone gets a checklist of who else needs to be in the loop, and each name gets a status, not a default assumption of yes.
- **RAG status is the top-line number, and it's computed, not guessed.** Green only if every milestone is on track and no handoff row sits at Assumed or Not yet within two weeks of its milestone date. One stuck row is enough to pull the whole project to Amber or Red, regardless of how the actual work is going.
- **Log major project decisions in `context/decisions-log.md`, not only in the project file.** The project tracker is the working day-to-day state. Decisions-log is the append-only record of what got decided, when, and why. A date slipping, a scope cut, a resourcing call, that's a decisions-log entry that references the project.
- **Never overwrite tracker history.** Append a new dated update block each time I check in, the same append-only discipline as decisions-log. If a milestone date changes, the old date stays visible with a note on why it moved.
- **A confirmed gap gets flagged same-day, not folded quietly into the next status update.** If a check-in reveals a team wasn't looped in, that's the headline of the update, not a bullet buried under "on track."

## Step 1: Intake the project

Before creating anything, get five things straight. If any of these is genuinely unclear, ask rather than guess:

1. **What** is this project, in one sentence a stakeholder outside it would understand.
2. **Why** it matters right now, what breaks or what's missed if it doesn't happen.
3. **Owner**: the one person accountable for the outcome. Not a team, a name.
4. **Milestones**: the real checkpoints with dates, not just a single end date.
5. **Every function this touches**: not just who's doing the work, but who needs to know about it, engineering, support/CS, sales, marketing, legal, finance, whoever the project's outcome lands on.

That fifth one is where handoff gaps start, and it's also where the RACI matrix comes from. For each workstream inside the project, decide who is Responsible (does the work), Accountable (owns the outcome), Consulted (has to weigh in before it moves), and Informed (needs to know, isn't asked). Do this at intake, before anything is moving. If I wait until the project is underway to assign it, everyone defaults to assuming someone's already got it covered, which is the exact blind spot this skill is built to close.

## Step 2: Create or open the tracker

Check `context/projects/[project-name].md` first. If it doesn't exist, create it with this structure:

```markdown
# [Project Name]

**Status:** [Scoping | In progress | At risk | Blocked | Done]
**RAG:** [Green | Amber | Red]
**Owner:** [name]
**Started:** YYYY-MM-DD

## Why this exists
[One paragraph, the problem or opportunity, and what happens if it slips]

## Milestones
| Milestone | Date | Owner | Status |
|---|---|---|---|

## RACI (per workstream)
Who SHOULD be involved, set at intake.
| Workstream | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|

## Handoff checklist (per milestone)
Whether the people named above are ACTUALLY looped in, updated every check-in.
| Milestone | Team/function | Looped in? (Confirmed / Assumed / Not yet) | Notes |
|---|---|---|---|

## Stakeholders
[Link to relevant context/people/*.md files]

## Open blockers
[Anything stuck, with what's needed to unstick it]

## Update log
[Append-only, dated, newest last, same discipline as decisions-log.md]
```

The RACI table and the handoff checklist do different jobs and both stay. RACI is the plan, decided once. The handoff checklist is the reality check, walked every time I look at the project. "Assumed" is a real, distinct status from "Confirmed", it means someone believes the team knows, but nobody has actually checked. That's the gap this skill exists to catch, and it's the input the RAG rollup reads directly.

## Step 3: Track status over time

At every check-in:

- Update milestone status and dates. If a date moves, note why in the update log, don't just overwrite the old date.
- Re-walk the handoff checklist. Anything still Assumed as its milestone gets closer is a live risk, not a formality to clean up later.
- Recompute RAG from the milestones and the handoff checklist, don't let it drift between updates. It's the first thing a skimmed status update should show, and it should never be more optimistic than the worst open handoff row.
- Cross-check `context/people/*.md` interaction logs for anything that reads like a gap signal, a stakeholder saying they "just heard about this," raising a concern for the second time, or asking a question that implies they weren't briefed. That's a handoff gap wearing a different sentence.
- If a blocker or gap is material enough to affect a date or a scope call, log it in `context/decisions-log.md` as an open decision, not just a line in the tracker. Decisions-log is what surfaces it to the founder/exec level, the tracker alone can go unread.

## Step 4: Surface gaps, don't wait to be asked

The default failure mode in a cross-functional project isn't a missed task, it's a team finding out too late. So the standing question at every check-in is: **has [team] actually confirmed they know, or am I assuming they do because someone competent is running their part, or because the RACI says they're Consulted?** A RACI assignment is not confirmation any more than confidence from a different team is. If the answer is Assumed, go find out before the next update, not after.

## Step 5: Close out

Mark the tracker Done only when every milestone is closed, every handoff checklist row is Confirmed, and RAG reads Green, not just complete on the doing side. Write a short closing note in the update log: what shipped, what nearly went wrong, and any handoff gap that got caught, so the pattern is visible next time a similar project starts.

## Intended Integrations

This repo tracks projects in local `context/projects/*.md` files because it's a demo repo, not a live company with existing tooling. In a real deployment I wouldn't invent a new place for this to live, I'd connect to wherever the team already looks:

- **Linear MCP**: if the company runs Linear for engineering work, the project and its milestones live there as the system of record, and I connect to it as an MCP server. The RACI table and handoff checklist become project properties or a linked doc, not a file I keep in sync by hand.
- **Notion project database**: a lighter-weight option when the company isn't engineering-heavy. One Notion database with Status, RAG, Owner, and the RACI fields as properties, one page per project, gets the same structure without Linear's issue-tracking overhead.

Either backend, the mechanism doesn't change: RACI decided at intake, a handoff checklist that tracks actual confirmation instead of assumption, and a RAG rollup computed from both. The local-markdown version here is the same skill running on a lighter backend, not a different design.

## Worked example: Nova Loop product relaunch

Sam Okafor (`context/people/sam-okafor-head-of-product.md`) owns the Nova Loop product relaunch. His file already shows the pattern this skill is built to catch: engineering is confident on the date, but he's raised twice that support/CS wasn't looped into launch-week comms planning. `context/decisions-log.md` (2026-06-22 entry) shows that gap got surfaced as an open, undecided risk rather than let ride on engineering's confidence.

Running this skill on that project looks like:

1. Intake: what = ship the relaunch; why = Priya's top current priority; owner = Sam; milestones = code freeze, comms plan finalized, launch day; functions touched = engineering, support/CS, marketing. RACI for the "launch-week comms plan" workstream: marketing is Responsible for drafting it, Sam is Accountable, support/CS is Consulted since their ticket queue should shape what the plan covers, engineering is Informed since they need the date, not a say in the plan.
2. Create `context/projects/nova-loop-relaunch.md` with those milestones, that RACI row, and a handoff checklist row for "Launch-week comms plan" x "Support/CS", status: **Not yet**, per Sam's own flag. RAG opens at **Amber**, a Not yet row inside two weeks of a milestone.
3. At the next check-in, that row is still not confirmed while the launch date is close. RAG moves to **Red**: the RACI says support/CS should be Consulted, but the handoff checklist shows they never actually were, that's the whole gap this skill is built to surface, not a footnote under "on track." Matches the decisions-log entry that left the date "open, decision needed by end of week" instead of quietly assuming engineering's confidence covers it.
4. Once support/CS confirms they've seen the comms plan, flip that row to **Confirmed**, RAG returns to Green once every other milestone is also on track, and log the date decision in decisions-log with the reasoning, not just the outcome.

## Rules

- Never mark a handoff checklist row Confirmed on an assumption, it means someone from that team actually said so.
- Never treat a RACI assignment as confirmation. It says who should be in the loop, not who is.
- Never let RAG sit at Green while a handoff row is stuck at Assumed or Not yet near its milestone, recompute it, don't inherit last update's color.
- Never let a project tracker go stale silently. If I haven't checked in on an active project in a while, that's worth flagging the same way a quiet stakeholder is.
- Don't create `context/projects/` speculatively. Create a project file when there's an actual project to track, not as scaffolding.
- Major decisions belong in `context/decisions-log.md` even if they're also noted in the project tracker. Don't make decisions-log the only place or the tracker the only place, they serve different readers.
- If scoping a project surfaces a gap that isn't mine to close (a decision only the founder/exec can make), say so and route it, don't sit on it hoping it resolves itself.

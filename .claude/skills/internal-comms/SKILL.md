---
name: internal-comms
description: Dispatches to the correct internal-comms format based on what's actually being asked for — 3P (Progress/Plans/Problems) updates, company newsletters, FAQ answers, status reports, leadership updates, and incident reports. Mirrors Anthropic's own first-party internal-comms skill pattern (github.com/anthropics/skills/tree/main/skills/internal-comms): recognize the request type first, then apply that type's exact format rules, never a generic comms template. Checks context/role-target.md for the company's stated tone/format convention and asks if none exists — this writes in the format the company already uses, not a house style I invent. Run for "write the 3P update", "draft the newsletter", "answer this FAQ", "status report for X", "leadership update on Y", or "incident report for Z".
user_invocable: true
---

# Internal Comms

Write the internal update in the format the request actually calls for, not a generic status report reshaped six ways. A 3P update, a company newsletter, and an incident report are different documents with different rules, even when they're pulling from the same week of work. The first job every time is naming which of the six formats this is, out loud, before drafting anything.

## Operating Rules (read first)

- **Six formats, six different rule sets.** 3P updates, company newsletters, FAQ answers, status reports, leadership updates, incident reports. Identify the type first (Step 1), then apply that type's format exactly as specified below — don't blend them.
- **This is team/company-facing, not single-stakeholder-facing.** These six formats go to a team or the whole company. An update addressed to one named board member, investor, or exec, shaped around that person's stated preferences, is `/stakeholder-update`'s job. If the real ask is "draft Elliot's investor email," redirect there instead of writing it here.
- **Leadership updates go out, not up.** The "leadership update" type here is a leader's own words broadcast to the team (a founder's post-relaunch-decision memo, a CEO's all-hands recap). A report going up to a leader is `/stakeholder-update`'s "internal leadership" shape, a different document.
- **Format convention isn't mine to invent.** Check `context/role-target.md` for a stated house style first. If it's silent, ask what convention this specific company already uses before drafting — don't default to a generic template because none was specified.
- **Source only from what's logged.** Pull from `context/decisions-log.md`, `context/goals.md`, and `context/people/`. A number or date not in the log gets flagged `[NEED: ...]`, never filled in with something plausible-sounding.
- **Never smooth a Problem, a risk, or an incident to make the update read cleaner.** Same rule as the root operating principles — surfacing drift early is the job, not writing around it.
- **Draft, don't send.** Every format here is a draft for the founder/exec's review. Nothing posts to Slack, goes out as a newsletter, or ships as an incident report without sign-off.

## Step 1: Identify the comm type

Match the request to one of the six types below. If it's ambiguous ("write an update on the relaunch" could be a status report or a leadership update), ask which one rather than guessing — the formats aren't interchangeable and picking wrong wastes the draft.

## Step 2: Confirm the company's format convention

Read `context/role-target.md`. If it states a house style (a specific 3P cadence, a newsletter template, a preferred incident-report structure), use that over the defaults below. If it's silent, ask what convention this company already uses before drafting. This skill writes in the format the company has, not one I'm importing from somewhere else.

## Step 3: Pull source material

Read `context/decisions-log.md` for the update window and `context/goals.md` for what's actually a priority right now. Check `context/people/` for anyone whose open items or interaction log feed this update. See Intended Integrations below for where this material would actually come from in a live deployment.

## Step 4: Apply the type's format

**3P updates (Progress / Plans / Problems)**
Strict one-line-per-section: Progress is one line, Plans is one line, Problems is one line (or "None" — the line stays even when empty, it never just disappears). Scale sections to team size: a small team gets one 3P block covering everyone; a larger org gets one block per function or team, not per individual unless that's explicitly asked for. If a line needs more than about 20 words to stay honest, that's a sign it belongs in a status report instead — don't let a 3P line become a paragraph.

**Company newsletter**
"We" voice — this is the company speaking, not a person reporting up (contrast the 3P's "Progress:" voice). Roughly 20-25 bullets total across the whole newsletter, not per section — that cap forces prioritization. Four sections, always in this order: Company Announcements (hires, policy changes, external news), Progress on Priorities (ties directly to `goals.md` and recent decisions-log entries — real progress, not filler), Leadership Updates (what leadership said or decided this cycle, stated plainly), Social Updates (team culture, events, shoutouts — the one section where a lighter tone is fine).

**FAQ answers**
Direct Q&A. State the question (cleaned up, not paraphrased into something vaguer), then answer it directly — no "great question," no preamble. Reserve this format for genuinely recurring questions; a first-time question still gets answered, just not filed as an FAQ entry yet. Source answers from `decisions-log.md` and prior comms — never invent a policy or number that isn't logged.

**Status reports**
Structured per project or initiative, not per person: Status (On track / At risk / Blocked), what changed since the last report, next milestone, owner. More room than a 3P line but still tight — no narrative padding. A single status report can cover several initiatives, but each gets its own Status line; never blend two projects into one verdict.

**Leadership updates**
Narrative, written in the leader's voice, going out to the team. Structure: what happened, why it matters to the team, what's next, and a direct ask if there is one. This is still a draft for that leader to review and approve before it posts — writing in their voice doesn't waive the sign-off gate.

**Incident reports**
Four sections, in this order: What happened, Impact, Resolution, Follow-up. Factual and blameless — describe what the system or process did, not who did it. Follow-up needs a named owner and a date, not "we'll fix it."

## Step 5: Draft

Write the update in the confirmed format, sourced only from Step 3. Every claim traces to a decisions-log entry, a goals.md priority, or a people/ file — anything else is `[NEED: ...]`, not a guess.

## Step 6: Self-check and present for approval

Before handing it over, check the draft against the type's rules by name (one-line-per-section actually held? bullet count actually near 20-25? Problems/Impact sections actually present, not smoothed away?). Present the draft to the founder/exec for review. This never posts, sends, or ships on its own — same gating-at-edge principle as everything else in this repo.

## Intended Integrations

This repo runs on flat files (`context/decisions-log.md`, `context/goals.md`, `context/people/`) so the demo works with zero external accounts. In a live deployment, source material would pull from where the actual signal lives, following the same sourcing pattern Anthropic's own internal-comms skill uses:

- **Slack** — pull from high-engagement channels (threads with heavy reply/reaction counts) as source material for what's actually worth including, rather than skimming every channel equally.
- **Google Drive** — docs with high view counts are a signal of what the team already considers important, and a natural source for newsletter and status-report content.
- **Email** — threads with a lot of replies flag live discussions and open questions worth surfacing, especially for FAQ candidates and incident follow-up.
- **Calendar** — non-recurring, high-importance meetings (including All-Hands) are a newsletter content source: what got announced or decided in a meeting that doesn't happen every week is exactly the kind of thing Company Announcements or Leadership Updates should carry.

The skill still drafts and self-checks locally first, every time. Approval happens before anything reaches Slack, a newsletter tool, or wherever incident reports actually get filed — the integration point is where an already-approved draft ships, not where it gets written.

## Worked Example: 3P Update — Nova Loop, week of 2026-06-24

Source: `context/decisions-log.md` (support-ticket spike investigation, relaunch date under review, runway model delivered) and `context/people/sam-okafor-head-of-product.md`. Nova Loop is a small team at Series A, so this scales to one 3P block per function, not per individual.

```
Nova Loop — 3P Update — Week of 2026-06-24

Product (relaunch)
- Progress: Engineering finished the relaunch build; confidence is high on the
  technical side.
- Plans: Decide by end of week whether the relaunch date holds or slips a week.
- Problems: Support/CS wasn't looped into launch-week comms planning — that
  gap, not engineering readiness, is why the date is under review.

Support / CS
- Progress: Confirmed ticket volume is up 30% since the relaunch, not a
  gradual trend.
- Plans: Root-cause finding due before Friday's board prep.
- Problems: Can't yet tell if this needs more headcount or a fix to
  launch-week comms — holding off on hiring until that's clear.

Fundraising
- Progress: Delivered the two-scenario runway model (current burn vs.
  planned hires) Elliot asked for on 2026-06-18.
- Plans: Feeds into the next board call.
- Problems: None open this week.
```

Each section held to one line per Progress/Plans/Problems. Problems stayed honest rather than smoothed — the relaunch-date risk and the hiring-decision risk are both still open in `decisions-log.md`, so they're still open here.

## Rules

- Identify the comm type before drafting anything — the six formats aren't interchangeable.
- Check `context/role-target.md` for a house style first; ask if it's silent, don't default to a generic template.
- 3P stays one line per section, every time, even when a section is "None."
- Newsletter stays in "we" voice, roughly 20-25 bullets total, in the fixed four-section order.
- Never fabricate a number, date, or outcome that isn't in `decisions-log.md`, `goals.md`, or a `people/` file — use `[NEED: ...]`.
- Never smooth a Problem, risk, or incident to make a draft read better.
- Every format here is a draft. Nothing posts, sends, or ships without the founder/exec's review and approval.
- If the real request is a single named stakeholder's update (board, investor, one exec), that's `/stakeholder-update`, not this skill.

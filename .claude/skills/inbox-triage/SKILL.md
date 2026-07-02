---
name: inbox-triage
description: Run a full triage pass across every message in the inbox (or a pasted/exported message set), not just what's urgent today. Classifies everything into three tiers, Reply Needed, Review, or Noise, using alias-aware routing that gets faster the more it runs, drafts Tier 1 replies matching the sender's known voice, and never sends or archives anything without explicit confirmation first. Use for a deep on-demand sweep. morning-briefing already covers the fast daily Tier-1-only scan.
user_invocable: true
---

# Inbox Triage

A full pass through everything sitting in the inbox, not just what's loud enough to show up in the morning brief. `morning-briefing` is the fast daily check, overdue items and stale relationships, read every morning in under a minute. This skill is the deeper sweep: read when the inbox has been left alone for a few days, or on request, and it goes through all of it, not just Tier 1.

The pattern is a three-tier classifier, borrowed from how the best inbox-triage tools actually work: **Tier 1 (Reply Needed)**, **Tier 2 (Review)**, **Tier 3 (Noise)**. Tier 3 never gets listed message by message, it gets summarized by count. That's what keeps a 40-message sweep readable.

## Operating Rules (read first)

- **Never send a drafted reply.** Every Tier 1 draft is a draft, sitting for approval. This holds even in a real deployment with a live send API wired in. Sending is the founder's/exec's action, not mine.
- **Never archive without explicit confirmation.** Batch the proposed archive list (usually Tier 3, sometimes handled Tier 2) and get an explicit yes before anything moves. No silent cleanup.
- **Alias routing is a shortcut, not a shortcut around judgment.** An alias only fires after a sender has been classified the same tier consistently. A sender's first few messages always get full classification.
- **Voice-match from `context/people/*.md` when a file exists.** Don't invent a communication style for someone who has a file, read it. Don't invent one for someone who doesn't, either, fall back to the plain, direct default voice and say so.
- **Never fabricate a fact in a draft reply.** If answering well needs a number, date, or commitment that isn't in `context/` or the source message, flag the gap in the draft rather than guessing at it.
- **This skill doesn't own `context/people/`.** If a message is from someone with no file who looks like a standing stakeholder, flag it, `relationship-crm` creates the file. Don't create one here.
- **This skill doesn't own `context/decisions-log.md`.** If a reply commits to something material, flag it for `decision-log` after the reply is approved. Don't log it here.

## Intended Integrations (not wired in this demo)

A real deployment of this skill connects to **Gmail MCP** for the inbox itself, drafts get written back as actual Gmail drafts, never sent via the API, so the human reviews and sends from inside Gmail like normal. **Slack MCP** is the optional second surface, DMs and @mentions run through the same three-tier pass so nothing that needs a reply is only sitting in Gmail's view.

Neither is wired in this repo. There's no live inbox connection here. Every run works from a message set the user pastes or exports into the session, subjects, senders, bodies, same as `morning-briefing` has no live calendar and works from what's pasted in. Treat that as the honest state of this demo, not a gap to paper over.

## Step 1: Gather the Message Set

Take the pasted or exported messages for this pass. Read `context/inbox-aliases.md` first if it exists, that's the running alias map from prior passes. If it doesn't exist yet, this is the first run, start it after Step 6.

## Step 2: Alias-Aware Pre-Classification

For each message, check its sender address or domain against `context/inbox-aliases.md`. If there's a confident entry (see Step 6 for what counts as confident), assign that tier immediately, no re-analysis. Everything else goes to Step 3.

## Step 3: Classify What's Left

Apply these rules to every un-aliased message:

- **Tier 1, Reply Needed:** the sender has an open item in `context/people/` or `context/decisions-log.md`, or is asking a direct question, or the message is time-sensitive. If missing this would be a real problem, it's Tier 1.
- **Tier 2, Review:** worth reading, informs a decision or needs an approval (an invoice, a proposal, a non-urgent request), but doesn't need a reply drafted right now.
- **Tier 3, Noise:** newsletters, automated notifications, receipts with nothing to approve, cc-only threads with no ask directed at me.

When a message is genuinely ambiguous between Tier 1 and Tier 2, ask rather than guess, that's the same rule `morning-briefing` uses for unclear calendar context.

## Step 4: Draft Tier 1 Replies

For each Tier 1 message, check `context/people/` for the sender.

- **File exists:** read Communication style, Standing context, and the most recent Interaction log entries before drafting. Match the tone and format they actually prefer (short brief, not long email, numbers before narrative, whatever the file says). Cross-check any open item the message might be closing.
- **No file exists:** draft in the plain, direct default voice from `CLAUDE.md`, and note in the draft's heading that no stakeholder file exists yet, flag it as a `relationship-crm` candidate if the sender looks like a standing relationship rather than a one-off.

Every draft sits under a `Draft, not sent` heading. None of these go anywhere without approval.

## Step 5: List Tier 2 Items

One line each: what it is, what action it needs (if any), and by when. No drafting unless asked. This is a scan list, not a queue of replies waiting to happen.

## Step 6: Roll Up Tier 3

Don't list Tier 3 messages individually. Summarize by count and category: `14 Tier 3 messages: 9 newsletters, 3 automated notifications, 2 receipts.` If one of them is a repeat sender not yet in the alias map, note it for Step 7.

## Step 7: Update the Alias Map

A sender earns an alias entry after landing on the same tier **3 times in a row** with nothing in between that contradicts it. Propose the new or updated entries to the user, don't write silently. Once confirmed, `context/inbox-aliases.md` uses this format:

```
| Sender / domain | Default tier | Confirmed | Last seen | Note |
|---|---|---|---|---|
| newsletter@example.com | Tier 3 | 3x | 2026-06-28 | Weekly digest, never needs action |
```

A tracked stakeholder's address (anyone with a `context/people/` file) can be aliased to Tier 1 immediately, no 3-message wait, they're already a known standing relationship.

## Step 8: Confirmation Gate

Present the full breakdown, Tier 1 drafts, Tier 2 list, Tier 3 rollup, and the proposed alias additions, before doing anything. Wait for explicit sign-off before sending a single draft or archiving a single message. If the user only approves part of it, act only on the approved part.

## Worked Example (Nova Loop, fictional)

A pasted set of 6 messages, run against the seeded `context/people/` files and `context/decisions-log.md`:

```
Inbox triage, 6 messages

Tier 1, Reply Needed (2):
1. Elliot Marsh <elliot@[fund]> — asking whether the runway scenario doc
   (delivered 2026-06-24 per the decisions log) reflects the latest hire
   plan, or if it needs a refresh before the next board call.
   Draft below, numbers-first per his file, no narrative padding.

2. Sam Okafor <sam@novaloop.com> — following up on the relaunch date
   decision, still open per the 2026-06-22 decisions-log entry and now
   past its "by end of week" target.
   Draft below, direct and detail-first per his file, references the
   support-comms gap he raised rather than assuming it's resolved.

[Draft, not sent — Elliot]
[Draft, not sent — Sam]

Tier 2, Review (1):
- Vendor invoice, project-management tool renewal, $340, due in 14 days.
  Needs approval before payment, not a reply. Flagging for sign-off, not
  drafting anything.

Tier 3, Noise (3): 2 SaaS product newsletters, 1 automated calendar
receipt. No action needed on any of them.

Alias map: no entry yet for the invoice sender or the newsletters, this
is their first appearance in a triage pass, nothing proposed for the
alias map this run.

Nothing sends or archives until you confirm.
```

## Rules

- Never send a drafted reply. Draft only, always.
- Never archive anything without an explicit, separate confirmation.
- Tier 3 is a count and a category breakdown, never a message-by-message list.
- Voice-match from the sender's `context/people/` file when one exists, plain default voice when it doesn't, and say which one was used.
- Don't fabricate a fact, a number, or a commitment in a draft. Flag the gap instead.
- An alias only fires after 3 consistent classifications, or immediately for a tracked stakeholder with a `context/people/` file.
- New stakeholders get flagged for `relationship-crm`, not created here. Material commitments get flagged for `decision-log`, not logged here.
- If a message is genuinely ambiguous on tier, ask, don't force a classification to keep the count clean.

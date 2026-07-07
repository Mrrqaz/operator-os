---
name: edge-approval-gate
description: The unconditional hold in front of anything that leaves the building under the founder's name or commits the founder to something. Classifies every action into draft-freely (internal briefs, agendas, notes, analysis) versus must-gate (external sends, founder-signed comms, commitments made on the founder's behalf, irreversible actions), and holds the must-gate ones for explicit sign-off. Operationalizes the root CLAUDE.md principle "autonomous inside the job, gated at the edge." Run when asked "can this go out," "does this need sign-off," "hold that," "is this mine to send," or before anything external ships under the founder's name.
user_invocable: true
---

# edge-approval-gate

A Chief of Staff drafts a lot that goes out under someone else's name, and commits to things on someone else's behalf. That is the job. It is also exactly where an over-eager assistant does damage: an investor email sent a beat early, a partner date confirmed before the founder actually agreed, a "yes" given to a third party the founder would have said "not yet" to. This skill is the single hold that stops any of that happening without an explicit sign-off.

It is the operational form of this repo's second operating principle: autonomous inside the job, gated at the edge. Inside the job, drafts flow freely, briefs, agendas, updates, project plans, analysis, none of it waits for anyone. At the edge, where an action would reach an outside party or bind the founder, everything stops here until the founder says yes. It holds; it does not draft the thing (that is `stakeholder-update` or `internal-comms`) and it does not judge whether the number in it is honest (that is `proof-boundary`).

## What counts as an edge action (always gated)

- **External sends under the founder's name.** An investor update, a board email, a reply to a partner or customer, anything a recipient will read as coming from the founder.
- **Commitments made on the founder's behalf.** Accepting a meeting or a speaking slot, agreeing a date, a price, or a deliverable with a third party, saying yes to a request the founder has not personally decided.
- **Irreversible or outward-facing actions.** Publishing anything, confirming something a counterparty will act on, any step that is hard to walk back once taken.
- **Anything that changes an outside party's understanding of a decision the founder has not ratified.** If it moves a decision from "being considered" to "settled" in someone else's mind, it gates.

Everything else, internal briefs, agendas, meeting prep, drafts held for review, notes into `context/`, flows freely. The gate is for the edge, not for the work.

## Operating Rules (read first)

- **Unconditional at the edge.** No draft is polished enough to send itself, and no commitment is obvious enough to make on the founder's behalf without a yes. There is no "trusted enough to skip the gate" tier.
- **The founder is the single approver.** A gate release comes from the founder (or whoever they have explicitly delegated it to in `context/role-target.md`), never from me inferring they would probably be fine with it.
- **I hold, I do not act.** A yes is a signal for the send or the commitment to happen through the founder's own channel and hand. I never send the email, confirm the date, or publish the thing myself.
- **Drafting and honesty checks are other skills.** `stakeholder-update` and `internal-comms` write the thing. `proof-boundary` checks its numbers are labelled honestly. A draft can be well-shaped and truthful and still sit held here until the founder signs off. Those are three different checks on the same artifact.
- **The evidence rides with the ask.** Every hold is presented as a clean yes/no with the action, its recipient, and one line of why it is held, so the founder decides in seconds, not by re-reading the whole thread.

## Steps

1. Take an action the system has prepared (a drafted send, a commitment about to be made, an outward step).
2. Classify it: is it an edge action (external send, commitment on the founder's behalf, irreversible/outward action), or internal work that flows freely?
3. If internal, let it move. If it is an edge action, hold it.
4. Build the approval ask: the action, its target, the linked source (the draft, the request it answers), and one line of why it is gated.
5. Present it to the founder and hold. Nothing proceeds while it waits.
6. On **yes**, hand the release back to the founder's own channel and log the decision in `context/decisions-log.md` if it settles something. On **no**, keep it held and note the call. I never act on either answer myself.

## Intended Integrations

Runs locally in this demo. In a live deployment the held actions would queue wherever the founder already works, a **Slack** approval message, a **Gmail** draft left unsent for one-click review, a **Linear/Notion** task flagged "needs founder sign-off," so the gate is a fast yes/no in a familiar tool, not a separate dashboard to check. The release still happens through the founder's own account and hand; the gate stages the decision, it is not a send rail.

## Worked Example (fictional)

Nova Loop, one week. Three prepared actions reach me. Two are edge actions and stop; one is internal and flows.

**1. Investor update to Elliot Marsh (external send, under Priya's name).** `stakeholder-update` drafted the 5-15 email and `proof-boundary` labelled its numbers. Both checks passed. It is still an external send under the founder's name, so it holds:

> **Send investor update to Elliot Marsh?** 5-15 monthly, drafted and truth-status checked. Why held: external send under Priya's name. Yes / No.

**2. Co-marketing date with a partner (commitment on the founder's behalf).** A partner asked to lock a launch date. I have the calendar and the draft reply, but agreeing a date binds Priya to a commitment she has not made yet, so it holds:

> **Confirm 2026-07-22 co-marketing date to [partner]?** Slot is free on Priya's calendar. Why held: commitment on the founder's behalf. Yes / No.

**3. The weekly 3P update to the leadership channel (internal).** `internal-comms` drafted it. It is internal, not a commitment or an outside send, so it flows without gating, though Priya still sees it like everything else.

In the two edge cases I held; I did not draft the email (that was `stakeholder-update`), I did not check its honesty (that was `proof-boundary`), and I did not send or confirm anything myself. On Priya's yes, the send and the date confirmation go out through her own channel, and the date, being a real commitment, gets logged to `decisions-log.md`.

## Adapted from

- **VERIFIED in the sibling system repos' builds:** HumanLayer (`github.com/humanlayer/humanlayer`), a purpose-built human-in-the-loop approval layer for agent actions, is the closest external analogue to this hold, a specific action paused for a human yes/no before it fires. Verified in those builds, not re-fetched here, so no fresh star figure is attached.
- **LIKELY:** the native Claude Code `PreToolUse` hook, which can gate an action before it runs by returning an "ask" decision to force human confirmation, and LangChain's human-in-the-loop middleware, which pauses an agent before a side-effecting call and waits for approve/edit/reject. Documented patterns, not re-fetched this session.
- The classification of what counts as an edge action for a Chief of Staff (external sends, commitments on the founder's behalf, irreversible outward actions) is **my own framing**, and it is the direct operational form of this repo's stated second operating principle in `CLAUDE.md`.

## Rules

- Every external send, every commitment on the founder's behalf, and every irreversible outward action passes through me. No exceptions, at any confidence. There is no auto-send tier.
- The founder is the single approver (or their explicitly delegated stand-in).
- I hold. I do not draft the thing (`stakeholder-update`, `internal-comms`) or judge its numbers (`proof-boundary`). A well-drafted, honest artifact still waits for sign-off.
- A yes routes back to the founder's own channel and hand. I never send, confirm, or publish myself.
- The ask carries its evidence: action, target, source, one line of why it is held. A real commitment that gets a yes goes into `decisions-log.md`.

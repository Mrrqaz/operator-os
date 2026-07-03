---
name: meeting-prep
description: Prep any meeting on the founder/exec's calendar by pulling attendee history from context/people and open items from context/decisions-log.md into a tight internal brief plus a shareable agenda. After the meeting, run debrief mode to capture what actually happened back into context/people and flag any real decision for the decision log, so the next prep on the same person is smarter than the last one. Run prep before any meeting, sync, call, or 1:1, and debrief right after it ends.
user_invocable: true
---

# Meeting Prep

The founder never walks into a meeting cold, and never lets what happened in it evaporate the moment it ends. This skill runs in two modes. **Prep**, before the meeting, pulls together who's in the room, what's actually open with them, and what the meeting needs to accomplish. **Debrief**, right after, captures what actually happened back into memory, so the next Prep on the same person or topic starts smarter than the last one did.

## Operating Rules (read first)

- **Read before you write, in both directions.** Prep reads every attendee's file and the decisions log before drafting anything. Debrief reads the current state of a file before appending to it. Don't guess at history, and don't overwrite state you haven't checked.
- **Brief, not biography.** Prep's output is what the founder needs to walk in ready, not a replay of the whole relationship. If a log has a dozen entries, only the last two or three plus anything still open matter for this meeting.
- **Two outputs, not one blob.** Prep produces an internal pre-read for the founder, and, when the meeting has outside attendees, a separate shareable agenda. Never let private context (communication-style notes, a stakeholder's frustrations, internal judgment calls) leak into anything meant to leave the room.
- **A missing person file is a flag, not a blocker.** If an attendee has no file in `context/people/`, say so in Prep and create the file in Debrief once there's something real to put in it.
- **Open items connected to this meeting come first.** Cross-reference `context/decisions-log.md` for anything still Open that touches this attendee or this topic. A prep that misses a live open decision has failed at its one job.
- **The agenda serves the founder's outcome, not a checklist.** Every internal agenda item maps to an open decision, a stated priority, or something the attendee raised last time. Don't pad it to look thorough.
- **Surface the decision, don't make it.** If the meeting needs a call the founder hasn't made yet, name the decision and the options in Prep. Never pre-decide it inside the brief and present it as settled.
- **Debrief writes to `context/people/`, not to `context/decisions-log.md`.** Follow the same append-only, newest-log-entry-on-top schema this repo's `relationship-crm` skill uses when touching a person file. If a real decision came out of the meeting, flag it for `context/decisions-log.md` and tell the user, don't write the entry yourself, that log's format and rules belong to the `decision-log` skill.

## Intended Integrations

This repo runs on local files, not live connectors, so today both modes read and write inside `context/` and rely on the user to paste in what a connector would otherwise fetch. If this were wired into a real stack:

- **Source of truth for people and decisions** stays `context/people/*.md` and `context/decisions-log.md`, or a Notion MCP mirroring the same schema (one page per stakeholder, one database for decisions) if the founder already runs their second brain in Notion. Local files win over Notion if the two ever disagreed, there's no Notion connection here to disagree with yet.
- **Schedule pull (Prep, Step P1)** would come from a Google Calendar MCP, answering who's in the room and when directly instead of asking the user to type it in.
- **Transcript source (Debrief, Step D1)** would come from Granola, Fireflies.ai, or Otter.ai rather than asking the user to recount the meeting from memory. Worth being honest about: no skill in this repo, or in the public meeting-prep patterns this one draws from, wires a transcript tool live end to end yet. Naming the intent, and the specific tools, is the differentiator over leaving debrief input as "paste your notes."

## Mode: Prep (Before the Meeting)

### Step P1: Identify the Meeting

Get the basics: title, time, attendees, and any existing agenda or description, from wherever the meeting is tracked, or directly from the user.

If attendees aren't named explicitly, ask before proceeding. Don't guess who's in the room from the title alone.

### Step P2: Pull Attendee History

For each attendee:

1. Check `context/people/` for a file matching their name.
2. If found, read the full file: role, relationship, communication style, standing context, open items, and the interaction log.
3. If not found, note the gap. Don't invent history to fill it, and don't create the file yet, that happens in Debrief once there's something real to write.

### Step P3: Check Open Decisions

Scan `context/decisions-log.md` for any entry where the status is Open and:
- the attendee is named as owner, or
- the attendee is referenced in the "why," or
- the topic overlaps the meeting's stated purpose.

Carry these forward as candidate agenda items. If a decision has stayed Open across more than one meeting without resolving, flag that explicitly. Letting something stall silently is the opposite of the job.

### Step P4: Build Two Outputs

**Internal pre-read** (for the founder only, never shared as-is):
- **Who's in the room.** One line per attendee: role, and what matters about how they operate.
- **What's open with them.** Pulled from their file's Open Items, plus whatever surfaced in Step P3.
- **What's likely to come up.** Based on the last one or two interaction log entries, not the full history.
- **Questions to walk in with.** The questions that move an open item forward or surface a risk before it becomes a surprise in the room.

**External-facing agenda** (shareable with attendees, when the meeting has outside participants):
- Three to five timed items, ordered by what actually needs a decision or update.
- Strip anything from the internal pre-read that's private color, a communication-style note, or an internal judgment call. An agenda item reads as a neutral topic, not as "push Sam because he's stalling."
- Skip this output for a pure internal 1:1 where there's no one outside the founder to send it to, note that it was skipped and why.

### Step P5: Present the Brief

Hand over the internal pre-read short and scannable, not a document, then the external agenda separately if one was built. See the worked example below for the shape.

Flag anything that changes the plan for the meeting. For example, if an attendee's open item conflicts with what the meeting was originally called to discuss, say so before the meeting starts, not after.

## Mode: Debrief (After the Meeting)

### Step D1: Capture What Happened

Get the outcome from wherever it exists: the user's own account, pasted notes, or a transcript (see Intended Integrations for where a transcript would come from in a live deployment). Don't proceed on a guess, if the user hasn't actually said what happened, ask.

Pull out, plainly, without interpretation dressed up as fact:
- What was decided, or explicitly left open.
- Any new commitment or ask, from either side.
- Anything that changes the standing picture of an attendee (a new priority, a role change, a shift in what they said their concern was).

### Step D2: Update Attendee Files

For each attendee with a file in `context/people/`:
1. Read the current file in full.
2. Insert one new Interaction log entry at the top: `- **YYYY-MM-DD** — [what happened, what they said, what they committed to].`
3. Check Open items: strike through anything this meeting resolved and append the resolution date, append anything new to the bottom.
4. Only touch Standing context if something durable changed, not off a single data point.

For an attendee with no file yet (flagged in Step P2), create one now using the template in `relationship-crm`, populated with what this meeting actually established.

### Step D3: Flag Decisions for the Decision Log

If the meeting produced a real decision, one someone could reasonably question later, that sets a precedent, or that touches risk, cost, or timeline, don't log it here. Tell the user it belongs in `context/decisions-log.md` and name the four fields the `decision-log` skill will need: Decision, Why, Owner, Status. Let that skill own the write and the format.

### Step D4: Close the Loop

Say plainly what changed for next time this attendee comes up in Prep. The value of a debrief is that Step P2's next run reads a file that already reflects this meeting, not one that's a week stale. If nothing meaningfully changed (a status-only sync, nothing new), say so and skip the ceremony.

---

## Worked Example

**Meeting:** Sync with Sam Okafor, 30 minutes, "relaunch date decision."

**Prep, Step P2, attendee history.** `context/people/sam-okafor-head-of-product.md`: Head of Product, detail-oriented, wants to see the plan before committing to a date, gets frustrated when scope shifts mid-sprint. Open item since 2026-06-22: needs a decision on whether the relaunch date holds given the support-ticket spike, or slips a week. Last interaction: engineering is confident, but support/CS wasn't looped into launch-week comms, and Sam wants that fixed before deciding.

**Prep, Step P3, open decisions.** `context/decisions-log.md` has a 2026-06-24 entry: hold off on approving support hires until the ticket-spike root cause is understood, recommendation due before Friday board prep. This sits directly upstream of Sam's question.

**Prep, Step P4/P5, the two outputs:**

```
Internal pre-read — Sync with Sam Okafor: relaunch date decision

Who: Sam, Head of Product. Wants the plan before the date, not the date
before the plan. Don't leave this half-decided, that's what frustrates him.

Open with Sam: whether the relaunch date holds or slips a week. Blocked on
support/CS actually being looped into launch-week comms, which Sam has
raised twice now.

Connected: the ticket-spike root-cause research is due before Friday board
prep and feeds directly into this.

Questions to walk in with:
- Has the comms gap Sam flagged on 6/22 actually closed, or just been discussed?
- If we hold the date, what's the fallback if support still isn't ready?

---

External agenda — Relaunch date sync (30 min)
1. Ticket-spike research: what we know so far
2. Support/CS involvement in launch-week comms: current status
3. Decide: hold the date, or slip a week
4. If slipping, what changes for the board update
```

**Debrief, after the meeting.** User reports: date held, support/CS now looped in as of this week, Sam confirmed he's comfortable moving forward.

**Debrief, Step D2, update to `sam-okafor-head-of-product.md`:**

```
## Interaction log

- **2026-07-02** — Relaunch sync. Support/CS now looped into launch-week
  comms, confirmed this week. Sam comfortable holding the original date.
- **2026-06-22** — Raised that engineering is confident but support/CS
  wasn't in the loop on the launch-week comms plan...
```

Open items: strike through the 2026-06-22 entry, append `— resolved 2026-07-02`.

**Debrief, Step D3, flag for decision log:** "This resolved the 2026-06-22 Open entry in `context/decisions-log.md`. Want me to tell you the fields so `decision-log` can close it out: Decision = relaunch date holds, Why = comms gap closed this week per Sam, Owner = Sam with Priya's sign-off, Status = Closed 2026-07-02?"

**Debrief, Step D4, close the loop:** Next time Sam is on the calendar, Prep's Step P2 reads a file where this is already resolved, not open. If the topic drifts back (a new spike, a new date question), Prep starts from what actually happened here instead of re-litigating it.

---

## Rules

- Never attribute a stated opinion or commitment to someone that isn't actually in their file or the log.
- Never resolve an Open decision inside a Prep brief. Surface it clearly, let the founder decide.
- Always check `context/decisions-log.md` in Prep, even for a meeting that looks purely relational. Decisions hide inside casual syncs too.
- Never put private, internal-only color into an external-facing agenda.
- Debrief is append-only. New interaction log entries go on top, resolved open items get struck through, nothing gets overwritten.
- Debrief flags decisions, it doesn't log them. `context/decisions-log.md` stays owned by `decision-log`.
- If an attendee's file is stale (last entry weeks old, or contradicts what's about to be discussed), say so in Prep rather than presenting it as current.
- If nothing meaningful happened in a meeting, a thin Debrief is correct. Don't pad it to look thorough.

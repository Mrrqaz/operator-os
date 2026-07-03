---
name: relationship-crm
description: Maintain the context/people/ stakeholder files after any interaction (meeting, call, message, transcript), and run a tiered staleness check to see who's gone quiet. Three run modes: all (full status on everyone), stale (only who's overdue), <name> (one person). Invoke after a meeting or call with a tracked stakeholder, when a new stakeholder shows up who isn't in context/people/ yet, or when asked who's gone quiet or what's overdue.
user_invocable: true
---

# Relationship CRM

Keep `context/people/*.md` as the institutional memory of every stakeholder relationship. One file per person. Every interaction adds to the record, nothing gets erased. This is what lets me walk into a meeting knowing what was said last time without asking the founder to re-explain it.

## Operating Rules (read first)

- **The schema is fixed.** Every person file uses exactly these sections, in this order: H1 title, one-line context blockquote (only on the fictional example files, drop it for real people), `**Role:**`, `**Relationship:**`, `**Communication style:**`, `## Standing context`, `## Open items`, `## Interaction log`. Don't add new top-level sections. Don't rename these.
- **Interaction log is newest-first.** New entries go at the TOP of the log, not the bottom. Match the existing files: read the top entry to see what happened most recently.
- **Open items is oldest-first.** New open items get appended at the BOTTOM, in the order they came up. This is a running list, not a log.
- **Never delete or overwrite a past entry.** If an open item resolves, don't remove it: strike it through (`~~text~~`) and append `resolved YYYY-MM-DD`. The fact that it existed and closed is itself institutional memory.
- **Staleness is tiered, not guessed.** Every person's `Relationship` field maps onto one of three fixed tiers with a fixed day count (Step 4). Don't eyeball "feels quiet": check the actual gap against the tier threshold.
- **This skill owns `context/people/`, not `context/decisions-log.md`.** If an interaction produced a real decision (not just an update), flag it to the user so it gets logged there too, in its own format. Don't duplicate decision-log entries inside a person's Standing context.
- **New stakeholder, new file.** Don't fold a new person into an existing file's Standing context. Every person tracked at this level gets their own file.

## Step 1: Figure Out What This Call Is

Three entry points into this skill:

1. **Log an interaction.** User hands me notes, a transcript, or a summary of a meeting/call/message and wants the relevant person file(s) updated. Go to Step 2.
2. **New stakeholder appeared.** Someone shows up in a meeting or is mentioned as a standing relationship and has no file yet. Go to Step 3.
3. **Staleness check.** User wants to know who's gone quiet or what's overdue. Go to Step 4 and pick the run mode:
   - `all`: full tier and open-item status on every file in `context/people/`, quiet or not. Use for a periodic review.
   - `stale`: only surfaces people who are actually past their tier threshold or carrying a stale open item. Use for a quick "who do I need to chase" check.
   - `<name>`: same two checks, one file only. Use right before a meeting with that person, or when the user names them directly.

If unclear which entry point applies, ask. Don't guess and write to the wrong file, and don't guess the run mode either: `stale` is the default for a bare "who's gone quiet," `all` only when the user actually wants the full picture.

## Step 2: Log an Interaction

1. Identify every person named in the source material who already has a file in `context/people/`. If someone mentioned has no file, branch to Step 3 for them first.
2. Read that person's current file in full before editing. The Standing context and Open items reflect the last known state; don't contradict them without explanation.
3. Write one Interaction log entry: `- **YYYY-MM-DD** — [what happened, what they said, what they asked for or committed to].` Keep it to what was actually said or decided, not my interpretation dressed up as fact. Insert at the top of the log.
4. Check Open items:
   - New ask or blocker surfaced? Append `- [YYYY-MM-DD] [the ask]` to the bottom.
   - An existing open item got addressed? Strike it through in place and append the resolution date, don't delete the line.
5. Check Standing context: only touch this if something durable changed (a new priority, a new working preference, a role or reporting-line change). A single data point isn't a pattern, don't rewrite Standing context off one interaction unless the person said it plainly (e.g. "I've handed the relaunch to Sam now").
6. If the interaction surfaced a real decision (not just a status update), tell the user this belongs in `context/decisions-log.md` too. Don't write it there myself unless asked.

## Step 3: Create a New Stakeholder File

1. Filename: `firstname-lastname-role.md`, lowercase, hyphenated, matching the existing convention (`priya-nakamura-ceo.md`, `elliot-marsh-investor.md`). The role suffix should be short enough to disambiguate, not a full title.
2. Use this exact template:

```markdown
# [Full Name] — [Role], [Company]

**Role:** [what they do, and their relevance to me/the founder]
**Relationship:** [contact cadence and nature: daily working relationship, monthly board update, ad hoc]
**Communication style:** [how they prefer to receive info and make decisions, if known yet]

## Standing context

- [durable facts: what they're focused on, what they care about, known preferences]

## Open items

- [YYYY-MM-DD] [anything currently waiting on me or on them]

## Interaction log

- **YYYY-MM-DD** — [the interaction that created this file]
```

3. If I don't yet know their communication style or standing context, write `Not yet established.` rather than guessing. An empty-but-honest field beats a fabricated one.
4. Don't touch `context/people/README.md`, it's a manually maintained index. Flag the user if a new file should be added to it.

## Step 4: Staleness Check

Two independent signals, checked per person, per run mode.

### Signal 1: contact cadence (tiered)

Map the person's stated `Relationship` field onto a fixed tier. This replaces any free-text judgment call: the tier and its day count are fixed, not a feeling.

| Relationship field reads like... | Tier | Flag if no Interaction log entry in... |
|---|---|---|
| Daily / near-daily working relationship | Tier 1 | 14 days |
| Weekly (standing sync, weekly 1:1) | Tier 2 | 30 days |
| Monthly (board update, monthly review) | Tier 3 | 60 days |
| Ad hoc / as-needed / quarterly | Tier 3 | 60 days |

Compare the top Interaction log date (newest entry) against today. If the gap exceeds the tier's day count, that person is stale on cadence.

If a `Relationship` field doesn't map cleanly onto one row (vague, mixed, or unwritten), don't force a tier. Flag it to the user as "cadence unknown" instead of guessing which bucket it belongs in.

### Signal 2: open item staleness

Independent of tier. Any open item whose date is more than roughly 10 business days old with no newer Interaction log entry that addresses it. Surface it: it's either been forgotten or quietly resolved without being logged.

### Run modes

- **`all`**: read every file in `context/people/`, report both signals for every person regardless of whether either trips. Full visibility, not just problems.
- **`stale`**: read every file, only report people where signal 1 or signal 2 actually trips. This is the default when the user just asks "who's gone quiet."
- **`<name>`**: read that one file, report both signals for that person only, whether or not they trip.

### Output format

Report both signals together per person, don't just say "stale." Say what's stale and since when, so the founder can judge whether it's a real gap or normal quiet. A Tier 1 stakeholder going quiet right after a call they seemed unhappy with is a different signal than a Tier 3 contact with nothing open and forty-five days of normal silence.

## Worked Examples

**Logging a call from raw notes:** User pastes rough notes from a call with Priya. I read `priya-nakamura-ceo.md`, see the last log entry was about the support-ticket spike, add a new top entry for today's call, check whether either of her Open items (the board deck, the Q2 KPI writeup) got closed out (strike it through if so), and leave Standing context alone unless she said something that changes her actual priorities, not just today's topic.

**New stakeholder appears:** Notes mention "looped in our new VP Sales, Dana Reyes, she'll own the pipeline number going forward." No file exists. I create `dana-reyes-vp-sales.md`, mark Communication style as `Not yet established.`, log the one interaction that introduced her, and tell the user a new file was created so they can add her to the people README if it should be tracked long-term.

**`stale` mode output, run on a hypothetical 2026-07-10 with nothing logged since the seeded data:**
```
Priya Nakamura (CEO, Tier 1 / daily, 14-day threshold) — last log entry 2026-06-24, 16 days ago. Flag: 2 days past threshold.
Open items from Priya (2026-06-20 board deck, 2026-06-24 Q2 KPI writeup) — both past 10 business days, no log entry references either closing out. Flag: unresolved or resolved-but-unlogged.
Sam Okafor (Head of Product, Tier 2 / weekly, 30-day threshold) — clean on cadence (18 days), but the relaunch-date item (2026-06-22) trips the open-item signal. Flag: unresolved or resolved-but-unlogged.
Elliot Marsh (Tier 3 / monthly, 60-day threshold) — clean on cadence (22 days), but the runway-scenario item (2026-06-18) is still open in his file while the decisions log shows it delivered 2026-06-24. Flag: resolved-but-unlogged, close it in his file.
```
Notice cadence alone only catches Priya, it's Signal 2, the open items, doing most of the work at this date. And Elliot's line is the same file-vs-log mismatch `morning-briefing`'s memory-hygiene check catches from its own angle: two skills, one gap, either one surfaces it.

**`<name>` mode:** User asks "how's it looking with Dana before I see her tomorrow." I read only `dana-reyes-vp-sales.md`, check her tier against today's date, check her open items, report both, and don't touch any other file.

## Intended Integrations

This skill runs entirely on local Markdown files in `context/people/`. That's a deliberate choice, not a placeholder for something better to come later. The tiered staleness pattern above comes from Mike Murchison's `/enrich` command in his `claude-chief-of-staff` repo, and that repo is also pure local files, not a live-synced platform. That's part of why it holds up as institutional memory: everything stays diffable, human-readable, and works without an API key or a subscription.

No external integration is required for this skill to function. If it were ever wired to a real external personal-CRM tool instead of local files, the one worth naming is **Dex** (getdex.com), a commercial personal-CRM product with an official Claude Code skill published at `github.com/getdex/agent-skills`. That would be the upgrade path if the founder wanted contact enrichment, reminders, or access outside this repo. It is not a requirement to make this skill work today.

## Rules

- Read the existing file before writing to it, every time. Never write blind.
- Append, never overwrite. History in these files is the point of the skill.
- Don't invent communication style, preferences, or standing context that wasn't actually said or clearly observed.
- Interaction log: newest entry on top. Open items: newest item on the bottom. Don't mix these up.
- A resolved open item stays in the file, struck through with a resolution date. It doesn't disappear.
- Staleness tiers are fixed by the `Relationship` field, not by feel. If the field is unclear, ask or flag "cadence unknown," don't invent a tier.
- Default staleness mode is `stale` (only surface actual problems). Only run `all` when the user wants full visibility, not just flags.
- Decisions that came out of an interaction go in `context/decisions-log.md`, flagged to the user, not folded into a person file.
- If genuinely unsure whether something is a durable Standing-context change or a one-off, ask rather than rewrite.

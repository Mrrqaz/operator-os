# Sample run: `morning-briefing`

> This is not a hypothetical. I ran `morning-briefing`'s six steps by hand against this repo's real `context/` files as they stand right now (no calendar pasted in, no live integration), on an assumed date of 2026-07-08. Nothing below was invented to look tidy, this is what the seeded files actually produce, including a real memory-hygiene gap the skill is built to catch.

## Working notes (Steps 1-5, before the brief gets compressed)

**Step 1.** `context/role-target.md` is still the unfilled template (`Status: Template — not yet filled`), so this run has no specific founder/company to anchor to. Proceeding generically, on the repo's own goals and cadence rules instead.

**Step 2.** No calendar was pasted in for today. Nothing to flag.

**Step 3.**
- `decisions-log.md`: two Open entries — the 2026-06-24 ticket-spike investigation (recommendation due before the 2026-06-26 board prep) and the 2026-06-22 relaunch-date decision (due "end of week," so by 2026-06-26). Both due dates have passed.
- `context/people/priya-nakamura-ceo.md`: open items are the 2026-06-20 board deck wait and the 2026-06-24 Q2 KPI writeup (also due before the 2026-06-26 board prep, also passed).
- `context/people/sam-okafor-head-of-product.md`: open item is the same 2026-06-22 relaunch-date decision, seen from his side.
- `context/people/elliot-marsh-investor.md`: open item is the 2026-06-18 runway-scenario doc. **Cross-checked against the decisions log: the matching 2026-06-18 entry there is Status Closed, delivered 2026-06-24.** His file was never updated after the fact. This is a live memory-hygiene gap, not a hypothetical one, flagging it.
- Staleness by cadence (14/30/60-day tiers, per `relationship-crm`): Priya (Tier 1, daily) last logged 2026-06-24, 14 days out, right at the threshold, with two overdue items riding on her. Sam (Tier 2, weekly) last logged 2026-06-22, 16 days out, under his 30-day threshold on cadence alone, but his open item is 16 days old with nothing newer addressing it, which trips the separate open-item-staleness signal. Elliot (Tier 3, monthly) is 20 days out against a 60-day threshold, fine on cadence, his issue is the file/log mismatch above, not staleness.

**Step 4.** Both overdue items sit on Standing priority 2 ("never let a special project stall silently") and priority 3 ("keep the relationship map current," which the Elliot mismatch also lands on directly). No `kpi-dashboard.md` exists yet, nothing to merge in from `okr-kpi-tracker`.

**Step 5.** No inbox was pasted in and nothing from Steps 3-4 crosses into genuinely Tier-1 urgent. Nothing to add.

## Step 6 output, exactly as printed

```
Morning brief — Wed, 2026-07-08

CALENDAR
No calendar provided today.

TASKS
1. Overdue: the relaunch-date decision (Sam's item, decisions-log 2026-06-22)
   was due "by end of week" on 2026-06-26. Twelve days past that with no
   status change logged. This is the top item, not a routine open one.
2. Overdue: Priya's Q2 KPI drift writeup and the support-ticket-spike root
   cause it depends on (decisions-log 2026-06-24) were both due before the
   2026-06-26 board prep. Same board-prep deadline as above, also missed.
3. Memory-hygiene gap: Elliot's people file still lists the runway-scenario
   doc as open (logged 2026-06-18), but decisions-log shows that exact entry
   Closed, delivered 2026-06-24. One of the two files didn't get updated.
   Close it in his file to match.
4. Missed-cadence flag: Sam, last logged contact 2026-06-22 (16 days), weekly-
   sync relationship. Not stale on cadence alone (30-day threshold), but the
   open item he's sitting on is 16 days old with no newer entry addressing
   it. He's also the one who raised the support/CS gap item 1 is stuck on.

GOALS
Standing priority 2 (never let a special project stall silently) and
priority 3 (keep the relationship map current) are both live right now, not
approaching: the relaunch decision has stalled past its own deadline, and
the Elliot mismatch is exactly the kind of relationship-map drift priority 3
exists to catch.

URGENT
Nothing surfaced. No live inbox integration in this demo, and nothing above
crossed into Tier-1.

FOCUS RECOMMENDATION
Get a real answer on the relaunch date from Sam and Priya before anything
else, it's now the most overdue item touching two standing priorities at
once. Close the Elliot file/log mismatch same day, it's a two-minute fix
sitting next to a much bigger one.
```

## Why this is here

The rest of this repo's worked examples live inside each `SKILL.md`, written as documentation. This one is different: it's a real execution of `morning-briefing`'s instructions against the actual files in `context/` at the time I ran it, not an authored illustration. The memory-hygiene gap it caught (Elliot's file vs. the decisions log) is real in this repo's current state, not staged for the example.

---
name: proof-boundary
description: The truth-status control that sits underneath every number and claim before it reaches the founder, the board, or an investor. Stamps each figure measured / projected / assumed / drafted, and enforces the two rules that keep an update trustworthy, never present a projection as a result, and never fill a missing number with a plausible-looking guess. Generalizes the [NEED:] discipline stakeholder-update already uses into a repo-wide labeling pass. Run before any board pack, investor update, KPI readout, or leadership brief goes to the founder, or when asked "is this measured or projected," "can we say we hit that," "mark the truth-status," or "keep this honest."
user_invocable: true
---

# proof-boundary

The fastest way a Chief of Staff loses a founder's trust is to hand them a number that turns out to be softer than it looked in the room. A projection read as a result. A figure someone said in a meeting, carried forward as if it were confirmed. A gap filled with something plausible so the update reads clean. This skill exists to make that impossible before anything reaches the founder, the board, or an investor.

It does one thing: it stamps every figure and every claim with its truth-status, and it holds two hard lines. It does not shape the document (that is `stakeholder-update`) and it does not set or track the KPI tree (that is `okr-kpi-tracker`). It labels what everything actually is, so the founder is never surprised by the real status of a number they already repeated to their board.

## The four labels

- **measured**: a confirmed figure from a source of record. A KPI dashboard row, a closed `decisions-log.md` entry, a finance number confirmed against the actual account. Present tense, real, checkable.
- **projected**: a modelled forward number. Runway under a hiring plan, a forecast, a scenario. A projection is never a result, and the label rides with it into every headline.
- **assumed**: an input taken on someone's word but not yet confirmed against a source. A number quoted verbally in a meeting, a figure a team lead "thinks" is right. Usable, but only if it is labelled, so the founder knows it still needs confirming.
- **drafted / for-approval**: a line prepared but not yet ratified by the founder. A commitment, a target, or a decision written into an update that the founder has not actually signed off on yet.

## Operating Rules (read first)

- **Every figure carries a status.** A number with no label does not go into an update. If I cannot say which of the four it is, it is not ready to show.
- **Never present a projection as a result.** The runway-under-the-plan number, the forecast, the scenario, all keep their `projected` label everywhere they appear, including in a one-line headline. This is the rule that stops a founder repeating a model to their board as if it already happened.
- **Never fill a gap with a plausible number.** A missing figure is marked `assumed` if it came from someone's word, or flagged `[NEED: what is missing]` if it came from nowhere. It is never estimated into something that reads confident. This is the same discipline `stakeholder-update` already applies with `[NEED:]`, generalized so every skill's output runs through it.
- **Source beats memory.** A number I am carrying from an old `decisions-log.md` entry that has not moved gets flagged as such, not restated as fresh.
- **I label, I do not shape or track.** Deciding the shape of the board deck or investor email is `stakeholder-update`. Setting and tracking the KPI tree is `okr-kpi-tracker`. I stamp the truth-status of the numbers those skills surface. A well-shaped, on-track update can still carry a mislabelled number, and catching that is my only job.

## Steps

1. Take each figure and each claim in the update the founder is about to see.
2. Assign its truth-status from the four labels, grounded in where it actually came from (a dashboard row, a decisions-log entry, a verbal input, a draft).
3. Scan for the two failures: a projection written as a result, and a gap filled with a plausible guess. Restate the honest status for each.
4. Confirm every `projected` figure keeps its label in every place it appears, including summaries and headlines.
5. Hand the labelled set back to `stakeholder-update` (which shapes it for the audience) and flag anything `assumed` or `[NEED:]` for the founder to confirm before it goes out.

## Intended Integrations

Runs on the local `context/` files in this demo, so no accounts are needed. In a live deployment the `measured` figures would be pulled from wherever the real numbers live, a **Google Sheet** or BI view for KPIs, a finance tool for cash and runway, so the label "measured" means checked against the actual source, not against a number typed into a doc. The labelling pass itself stays local and runs before anything reaches Slides, Docs, or a stakeholder.

## Worked Example (fictional)

Nova Loop, board pack for the 2026-Q2 meeting. `stakeholder-update` has drafted it; I run the truth-status pass before Priya (CEO) sees it.

```
Nova Loop - Board Pack Q2 - truth-status pass

- Cash / runway: 14 months          [measured]  - finance-confirmed, decisions-log 2026-06-24
- Runway under the 4-hire plan: 9mo  [projected] - scenario model, NOT a committed number
- NRR: 108%                          [assumed]   - quoted by Sam in the 06-30 sync, not yet
                                                   confirmed against the billing export
- Q3 bookings target: on track       [drafted]   - Priya has not signed off this line yet
- CAC payback: [NEED]                             - not reported this cycle, do not estimate
```

The one figure I blocked: the deck's headline read "9 months of runway," pulled from the hiring-plan scenario. That is a `projected` number written as if it were the current cash position. I restated it as "14 months today; 9 months if we run the 4-hire plan," so Elliot Marsh (investor) reads the real position and the scenario as two different things, not one soft number. NRR stayed in as `assumed` with a note to confirm against billing before the meeting, not dropped and not laundered into looking measured. CAC payback was missing, so it is `[NEED]`, never a filled-in guess.

I labelled every figure and held the two lines. I did not decide the deck's shape (that is `stakeholder-update`) and I did not set the KPI targets (that is `okr-kpi-tracker`).

## Adapted from

- The audit-trail / provenance idea, that every reported figure keeps a reference to where it came from and can be traced back to its source, is a pattern the sibling system repos in this set reuse per row. **VERIFIED in those builds**, not re-fetched here, so no star figure is attached. I reused its "every number carries its origin" backbone and turned it into a four-way truth-status label.
- The never-present-a-projection-as-a-result and never-fill-a-gap rules are **my own design**, carried from the same control in those system repos. This skill also generalizes a discipline already in this repo: `stakeholder-update`'s `[NEED:]` rule, applied here as a labeling pass every skill's output runs through, not just investor emails.

## Rules

- Every figure and claim carries a truth-status (measured / projected / assumed / drafted). An unlabelled number does not reach the founder.
- A projection is never shown as a result. The label holds everywhere, including headlines.
- A missing number is `assumed` (with a source to confirm) or `[NEED:]`, never estimated into something that reads confident.
- I label. I do not shape the document (`stakeholder-update`) or set and track the KPIs (`okr-kpi-tracker`).
- A stale decisions-log figure gets flagged as stale, not restated as fresh news.

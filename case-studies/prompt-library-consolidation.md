Case Study: Consolidating a Prompt Library Without Losing Capability compiled by Steven Illari @ illartech.com ©

## Problem

I maintain a personal library of reusable AI prompts, organized by category (strategy, marketing, productivity, analysis, and so on), with a dispatcher pattern in front of it: describe the task in plain language, and a router prompt recommends which library entries to use and in what order.

Grown organically over time, the library reached 89 entries. Organic growth has a predictable failure mode: near-duplicate prompts that do the same job with slightly different framing, entries built around a speculative idea rather than a concrete output, and no consistent way for a new or time-pressed user — human or the dispatcher itself — to find the right entry without reading the whole library first.

The easy fix is "just delete some." The problem with that fix is you can't tell, after the fact, whether you deleted a duplicate or a capability.

## Criteria

I treated every entry as needing one of three dispositions, and required a reason for each:

- **Merge** — two or more entries solve the same underlying job with different surface framing. Collapse them into one entry with selectable modes (e.g., four separate "write an email for X context" prompts become one entry with a mode parameter), so the interface shrinks without the coverage shrinking.
- **Absorb** — an entry's output is already a strict subset of what a broader entry produces. Fold it in; keep the broader entry as the surviving one.
- **Remove** — an entry has no verifiable output or success criteria attached to it (open-ended/speculative rather than task-bound). Cut it, but log what was cut and why.

Every entry needed a disposition on this list before it could leave the library — "seems redundant" wasn't sufficient on its own; it had to map to merge, absorb, or remove with the reason stated.

## Evaluation Method

The test wasn't the smaller number — it was whether anything was lost. For every entry removed or merged, I checked it against the surviving library:

1. **Traceability** — every one of the 22 removed/merged entries has a one-line disposition record: what it became, or why it had no output-bound justification to keep. An entry with no disposition record is a bug in the consolidation, not a completed one.
2. **Zero unique-capability loss** — merges and absorptions preserve the underlying job as a *mode* of a surviving entry, not a deletion. If a removed entry couldn't be mapped to a mode or an absorption, it had to justify itself as "remove" on its own — no dumping ground category.
3. **Navigation, not just a smaller list** — the real deliverable was a "what do I need → where do I start → what do I chain it with" lookup table sitting in front of the library. A shorter list that still requires reading start-to-end hasn't solved the actual problem, which is retrieval speed, not entry count.

## Outcome

89 entries down to 67 (~25% reduction), with every removed or merged entry individually dispositioned rather than silently dropped. The role-specific subset went through the same process, 19 roles down to 16, using the identical merge/absorb/remove test.

The bigger result wasn't the count — it was that the dispatcher pattern in front of the library got more reliable once the entries it was routing across stopped overlapping. A router choosing between two near-duplicate prompts has no principled way to pick; a router choosing between 67 non-overlapping ones does.

Same throughline as the delegation-rubric case study: consolidation only counts as an improvement if you can show what happened to everything you removed, not just how much shorter the list got.

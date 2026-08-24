Framework: Consolidation Disposition Rubric compiled by Steven Illari @ illartech.com ©

A method for shrinking any collection of near-duplicate items (prompts, SOPs, templates, feature flags, docs) without silently losing coverage. Built and validated on a personal prompt library — see [Consolidating a Prompt Library Without Losing Capability](../case-studies/prompt-library-consolidation.md) for the applied version and outcome data.

## When to use this

Any collection that grew organically and now has overlap: near-duplicate entries doing the same job with different framing, entries built around a speculative idea rather than a concrete output, and no fast way for a reader — human or an automated router in front of the collection — to find the right one without scanning everything.

The trap this avoids: "just delete some" feels like progress, but you can't tell afterward whether what got cut was a duplicate or a capability.

## The rule

Every entry gets exactly one of three dispositions before it can leave the collection. "Seems redundant" is not a disposition on its own — it has to resolve to one of these, with a reason logged:

| Disposition | When it applies | What survives |
|---|---|---|
| **Merge** | Two or more entries solve the same underlying job with different surface framing | One entry with selectable modes — interface shrinks, coverage doesn't |
| **Absorb** | An entry's output is already a strict subset of a broader entry's | The broader entry, unchanged; the narrower one is gone |
| **Remove** | An entry has no verifiable output or success criteria attached — speculative rather than task-bound | Nothing — but the cut and the reason are logged |

## Verification checklist (run after, not instead of, the cut)

- [ ] **Traceability** — every removed or merged entry has a one-line disposition record: what it became, or why it had no output-bound justification to keep. No record = the consolidation isn't done, it's a bug.
- [ ] **Zero unique-capability loss** — every merge/absorb maps to a mode of a surviving entry, not a deletion. An entry that can't map to either has to justify itself as "remove" on its own; there's no dumping-ground category.
- [ ] **Navigation, not just a smaller count** — the actual deliverable is a lookup layer ("what do I need → where do I start") in front of the collection. A shorter list that still requires reading start-to-end hasn't fixed the real problem, which is retrieval speed, not entry count.

## Why the count isn't the point

A collection of near-duplicates degrades whatever sits in front of it, not just human readers — a router choosing between two near-identical entries has no principled way to pick. The rubric works because it forces every removal through the same three-way test and leaves a record, which is what turns "I'm fairly sure we kept everything" into something checkable after the fact.

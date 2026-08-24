# Portfolio — Steven Illari

Case studies on evaluation, rubric design, and AI-system delegation, drawn from production work at illartech.com. Each writeup follows the same structure: problem → criteria → evaluation method → outcome.

More context and full-length posts: [illartech.com](https://illartech.com)

## Case Studies

- [Designing a Delegation Rubric for AI Subagents](case-studies/subagent-delegation-rubric.md) — routing, cost-tiering, and review-verification criteria for a multi-agent system, including what a review procedure that only reads output (vs. verifies it) misses.
- [Consolidating a Prompt Library Without Losing Capability](case-studies/prompt-library-consolidation.md) — curation criteria and a traceability method for cutting a personal prompt library by 25% with zero unverified capability loss.

## Frameworks

Conceptual templates only — the underlying production prompts and configs stay private.

- [Delegation & Routing Rubric](frameworks/delegation-routing-rubric.md) — when to hand work off, which cost tier to route to, and how to make review resistant to confident-but-unverified output.
- [Consolidation Disposition Rubric](frameworks/consolidation-disposition-rubric.md) — a merge/absorb/remove test for shrinking any collection of near-duplicate items without silently losing coverage.

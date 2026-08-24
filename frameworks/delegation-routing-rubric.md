Framework: Delegation & Routing Rubric compiled by Steven Illari @ illartech.com ©

A decision structure for when to hand work off to another agent/model/worker instead of doing it yourself, which cost tier to route to, and how to make review resistant to confident-but-unverified output. Built and validated on a multi-agent AI system — see [Designing a Delegation Rubric for AI Subagents](../case-studies/subagent-delegation-rubric.md) for the applied version and outcome data.

## When to use this

Any system where a cheaper/faster path exists alongside a more capable/expensive one, and picking wrong in either direction has a cost — over-routing burns budget on trivial work, under-routing burns your own time/context on work that should've been offloaded.

## 1. Routing ladder (first match wins)

Work top to bottom. Stop at the first "yes."

| # | Question | If yes |
|---|---|---|
| 1 | Does a purpose-built tool already cover this? | Use it. Don't route to a general worker. |
| 2 | Does an existing specialist (agent/role/process) cover it? | Use it — a generalist standing in for a specialist is a mis-route. |
| 3 | Does it need multi-step work with no specialist fit? | Route to a general-purpose worker. |
| 4 | None of the above clearly fits? | Keep it. Don't delegate. |

## 2. Cost-tier gate

Route to the cheapest tier **only if every box below checks true.** Any single unchecked box means the mid tier is the default, not the cheap one.

- [ ] Pure pattern-matching — no judgment call about what the content means
- [ ] A wrong result is obvious in the output itself (wrong file, wrong count, malformed structure)
- [ ] Touches at most one framework/convention/domain
- [ ] Success can be stated in one sentence with zero background context

## 3. Review-tier rule

The reviewer runs at the **same or higher capability tier** as whoever produced the work under review, for anything flagged high-stakes. A cheaper reviewer systematically under-catches a more capable producer's hardest errors — it isn't equipped to see what it's checking for.

## 4. Making review itself checkable

1. **Verify by command, not by reading.** A claim ("this is fixed," "this is backed up") is unverified until the reviewer has *run* the check that proves it. Reading a claim is proofreading, not review.
2. **Track recall, not just precision.** Precision — how many flagged findings were real — is the easy number to look good on. Recall — how many real problems got caught at all, including ones later found by other means — is the number that exposes a reviewer that skims.
3. **State the drill budget up front.** How deep the review goes and how many passes, decided *before* starting. An unbounded review has no natural stopping point.

## What stays out of this template

The actual tools, models, and cost figures used to fill in tiers 1–4 are specific to one production system and aren't reproduced here — the structure above is what transfers.

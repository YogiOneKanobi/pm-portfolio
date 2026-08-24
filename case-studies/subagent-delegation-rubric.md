Case Study: Designing a Delegation Rubric for AI Subagents compiled by Steven Illari @ illartech.com ©

## Problem

I run a multi-agent AI system where a parent process delegates work to specialized subagents (research, review, mechanical cleanup). Left unmanaged, delegation drifts two ways: over-spawning agents for tasks a human — or the parent itself — should just do directly, or under-spawning and burning parent context on work that should've been offloaded.

Worse: a review pass I ran with a cheaper model over higher-effort, higher-cost output accepted several unverified claims at face value — "this is backed up," "this is the only copy" — without checking. The prose read as confident. It wasn't checked. That gap is the same failure mode AI-eval platforms are built to catch: a model producing plausible-sounding output that doesn't hold up under verification.

I needed a rubric that decided, up front, three things: when to delegate at all, which cost tier to route to, and how to make review itself resistant to the "confident but unverified" failure.

## Criteria

Built as a decision rubric, not a style guide — every rule resolves to a checkable yes/no:

**Routing (first-match-wins ladder):**
- Does a built-in, purpose-fit tool already cover this? Use it.
- Does an existing specialized reviewer/agent cover it? Use it — a generic agent standing in for a specialized one is a mis-route.
- Does it need multi-step research with no specialized fit? General-purpose agent.
- None of the above clearly fits? Stay in the parent process. Don't spawn.

**Cost-tier gate (cheapest tier only if ALL true):**
- Task is pure pattern-matching — no judgment call about what the content means
- A wrong result is obvious in the output itself (wrong file, wrong count, malformed syntax)
- Task touches at most one framework/convention
- "Success" can be described in one sentence with zero project background

Any box unchecked → route to the mid-tier model by default, not the cheap one.

**Review tier:** for anything flagged high-stakes, the reviewer runs at the *same or higher* capability tier as whoever produced the work. A cheaper model reviewing a more capable model's output systematically misses that model's hardest errors — it under-indexes on catching subtle failures because it's less equipped to spot them.

## Evaluation Method

The rubric's real test was the review procedure, not the routing logic. Two rules:

1. **Verify by command, not by reading.** A claim like "this is backed up" or "the fix is in place" is unverified until the reviewer has *run* the check that proves it — not just read a comment asserting it. A reviewer that only reads prose is not reviewing; it's proofreading.
2. **Track recall, not just precision.** I started logging every review pass: findings returned, true positives, false positives, and — critically — misses caught later by other means. Precision (how many flagged findings were real) is the easy number to look good on. Recall (how many real problems got caught at all) is the number that matters, and it's the one that exposes a reviewer that skims instead of checks.

I also required every review to state its drill budget *before* starting — how deep it would go, how many passes — because an unbounded review has no natural stopping point and will burn cost long past the point of diminishing findings.

## Outcome

One review pass under this rubric, checking gate code that governs what content ships to a live site, returned 9 findings — all 9 confirmed true positives, including a check that reported "success" while having inspected nothing (an empty-input pass with no data behind it), and a status-field typo that would have silently downgraded a signed-off item back to draft.

A later pass under the same rubric caught 5 of 6 real issues — precision 5/5, recall 5/6. The one miss was a defect sitting under a comment that *asserted* it was already fixed. The lesson that shipped back into the rubric: a comment claiming a fix is not evidence of a fix, and "verify by command" has to apply even when the file itself is telling you not to bother.

That's the throughline I'd bring to evaluator/rubric-design work: build criteria that are checkable, not just describable, and measure the checker's own miss rate — not only what it catches.

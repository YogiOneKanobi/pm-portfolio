Framework: Model Output Promotion Rubric compiled by Steven Illari @ illartech.com ©

A decision framework for moving AI-generated output from an untrusted working area into a tracked, published, or executable system. Built and adversarially tested on a local-model workflow. See [Quarantining Untrusted AI Output](../case-studies/model-output-quarantine.md) for the applied version and outcome.

## When to use this

Use this whenever model output crosses a trust boundary: becoming a committed file, entering a knowledge base, feeding a tool-bearing agent, updating a customer record, or triggering an external action.

If the output is only displayed to a human and discarded, the full promotion path may be unnecessary. The moment another system will trust or act on it, the boundary applies.

## The promotion decision

Promotion requires every row below to pass. A warning is not a pass, and “the model is local” does not change the trust class.

| Check | Required evidence | Fail behavior |
|---|---|---|
| **Confinement** | Candidate is a regular file or value inside the expected untrusted boundary | Reject paths, symlinks, caller-selected destinations, and ambiguous input |
| **Byte identity** | The bytes used downstream match the bytes inspected | Re-read or re-hash immediately before use; stop on any change |
| **Sensitive-data result** | Secret/PII signals are known and recorded | Stop on secrets; route uncertain or restricted content away from tracked/public systems |
| **Explicit classification** | A named actor or deterministic policy selected the allowed destination class | No classification means no promotion |
| **Destination policy** | The selected class is permitted to enter that destination | “Reviewed” is not synonymous with “publishable” or “committable” |
| **Provenance** | The decision is tied to the exact output bytes and cannot be silently rewritten | Missing, malformed, or unverifiable evidence blocks promotion |
| **Downstream authority** | The next system grants only the minimum action needed | Do not turn approved text into unrestricted tool authority |

## Evaluation checklist

- [ ] Positive control: a known-valid candidate can pass.
- [ ] Mutation arm: changing the candidate after inspection blocks use.
- [ ] Path arm: rename, traversal, symlink, and type-change attempts fail.
- [ ] Parser arm: empty, malformed, and unusual filenames or records fail safely.
- [ ] Provenance arm: unsigned, altered, and mismatched records fail.
- [ ] Exact-bytes arm: the downstream check reads the committed or executed bytes, not a mutable proxy.
- [ ] Independent review: each claimed bypass is reproduced, then becomes a regression case.

## What each control does not prove

| Control | What it proves | What it does not prove |
|---|---|---|
| Scanner | A known pattern was or was not detected | Absence of contextual PII, manipulation, or hallucination |
| Human review | A person accepted the content for a stated purpose | That the reviewed bytes are the bytes later used |
| Hash | Two byte sequences match | That either sequence is safe or correct |
| Signature | An authorized key signed the record | That the signer made a good decision |
| Sandbox | Code ran within a constrained environment | That output is safe after it leaves that environment |

## Stop condition

No output crosses the boundary unless the system can state: **these exact bytes were inspected, assigned this trust class, approved for this destination, and recorded by this authority.** If any part is unknown, keep the output quarantined.

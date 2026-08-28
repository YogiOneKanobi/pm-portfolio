Case Study: Quarantining Untrusted AI Output compiled by Steven Illari @ illartech.com ©

## Problem

I run local AI models as workers inside a larger production system. Their output can be useful, but it is not trusted just because the model runs on hardware I control. A response can still echo sensitive material, carry prompt-injection instructions, invent a destination, or change between inspection and use.

The unsafe path was also the convenient one: let a model write a file, scan it, then move it into the normal workflow. That design treated a clean scan as proof that the file was safe and treated a local model as a trusted author. Neither assumption held.

I needed a promotion system that answered three questions before any model-produced file could move downstream: are these the same bytes that were inspected, what trust class do they belong to, and is there durable evidence that the decision happened?

## Criteria

I defined the boundary as a promotion decision, not a malware scan. A file could move only when every criterion held:

- **Confinement:** the candidate had to be a regular file inside the untrusted landing zone. Paths, symlinks, and caller-supplied destinations were rejected.
- **Byte identity:** the bytes inspected had to be the bytes promoted. A second hash immediately before the move detected swap-under-scan races.
- **Sensitive-data handling:** secret and PII checks informed the decision, but a clean heuristic scan never counted as proof that contextual sensitive data was absent.
- **Explicit classification:** unrestricted output could enter the tracked tier; restricted output stayed local and untracked. “Promoted” did not automatically mean “safe to commit.”
- **Provenance:** every promotion needed a signed record tied to the exact output bytes.
- **Fail-closed behavior:** a missing key, unreadable record, malformed input, or uncertain classification stopped the move.

The design deliberately kept the model out of the decision. The model produced text. Trusted code chose the filename, destination, classification path, and whether the result could move at all.

## Evaluation Method

Happy-path tests were not enough for a control whose job was to resist bypass. I evaluated each boundary with a positive control and an adversarial arm:

1. Confirm a valid candidate could pass, so the gate was not simply unusable.
2. Change the candidate after inspection and confirm promotion stopped.
3. Attempt alternate filesystem paths: rename into the trusted tier, change a regular file into a symlink, and use unusual filenames that break naive shell loops.
4. Attempt to create or alter provenance without the signing authority.
5. Verify the exact staged bytes at the version-control boundary, not the mutable working copy.
6. Send the implementation through independent security review, with reviewers required to demonstrate each finding rather than only describe a hypothetical weakness.

The test record counted bypasses found after my own suite had passed. That made the process measure the gate's blind spots instead of rewarding it for the cases I already knew to write.

## Outcome

Independent review found five exploitable paths that my passing tests had missed: a scan-to-move race, a filename-driven denial of service, a rename into the trusted tier, a regular-file-to-symlink type change, and a filename-based exemption that could carry unreviewed bytes. All five were reproduced, fixed, and added to the regression checks before release.

The finished control separated three ideas that are often collapsed into one:

- A scanner reports signals; it does not assign trust.
- A human classification authorizes a route; it does not prove byte identity.
- A signed provenance record proves what passed; it does not make the content correct.

That separation is the transferable result. Safe AI automation does not begin by trusting a better model. It begins by treating model output as an untrusted input to a small, explicit promotion decision whose failure modes can be tested.

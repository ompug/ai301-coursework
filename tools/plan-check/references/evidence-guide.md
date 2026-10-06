# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives:** In eval mode, compare the candidate plan's cause with the issue context and the complete repro-evidence block, including controls and limitations. In live mode, use the diagnosis and quoted evidence in `plan.md`, then verify it against the student's posted reproduction comment on the issue.

**What good looks like:** The cause explains the exact observed failure and remains consistent with every relevant control. It may be a bounded internal hypothesis when the plan names how tracing or testing could disprove it; the reproduction is not required to prove unseen internals. A limitation narrows the claim, and neighboring defects stay separate.

## Scope

**Where it lives:** Read the candidate plan's change, exclusions, file or subsystem list, risks, and deviations against the issue request. In live mode, also inspect repository layout only far enough to confirm that the named areas exist.

**What good looks like:** The plan contains one coherent fix plus the focused tests or support edits needed to verify it. It explicitly leaves adjacent defects and optional redesigns out. A plan that bundles migration, cleanup, new features, or broad refactoring fails even if its core fix is valid.

## Executability

**Where it lives:** Use the candidate plan's approach, named files or components, sequence, and unknowns. The plan comment can summarize these facts but cannot supply a material decision missing from the plan.

**What good looks like:** The behavior, boundary, and affected components are chosen, and material steps are ordered when order matters. A stranger can start without asking which of several mechanisms to use. Exact symbols, confirmation that named tools support a standard argument, or ownership between two named adjacent components may remain when a bounded trace and decision criterion are stated.

## Test plan

**Where it lives:** Compare the candidate plan's test section with the repro block's commands, input, failing output, control, and expected result. In live mode, use the quoted reproduction evidence and posted report.

**What good looks like:** The plan re-exercises the affected code path or adds a focused regression test for the same trigger, and states the exact post-fix result. Broader checks may supplement that proof. "Run tests," "works," or "feels faster" without a distinguishing observable is insufficient.

## Honesty

**Where it lives:** Read the candidate plan's risks, unknowns, environment limits, exclusions, and `Deviations` section when present. Compare certainty in the plan and comment with what the reproduction actually established.

**What good looks like:** Known limits are stated at the point they constrain the claim, unresolved facts are bounded rather than disguised, and a build change is recorded with its reason. No unknown may defer the plan's core implementation decision.

## Comms

**Where it lives:** In eval mode, read the candidate comment against thread highlights and the repo-facts block's contribution rules and applicable AI-use policy. In live mode, read the full issue thread and the repository's contribution guide and comment-specific instructions.

**What good looks like:** The comment gives the author's own evidence-based diagnosis, bounded approach, and verification intent. It acknowledges explicit maintainer direction or prior work that changes the plan. In eval mode, enforce a repo-facts rule requiring disclosure of all AI use even when the candidate comment is silent. In live mode, apply disclosure only when the repository states that it governs this comment; do not import requirements from an unrelated template.

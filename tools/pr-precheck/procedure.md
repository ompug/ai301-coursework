# Procedure: how this tool grades a PR package

## Read order

1. Select live or eval mode. In live mode, enforce `scope.md`, then read the issue, full thread, PR template, and contribution guide. In eval mode, read the issue context, thread highlights, and repo-facts block. Record every explicit issue outcome, maintainer direction, template ask, check, and policy requirement before reading the candidate's claims.
2. Read the accepted plan and reproduction context next. Record the planned scope, excluded work, named files or areas, test plan, before-state observable, expected after-state, controls, and every entry under `## Deviations`.
3. Read the committed diff and commit list before the PR description. Inventory every changed file and material hunk, then mark the intended fix, tests, documentation, debris, and any change with no plan source.
4. Read the test evidence. Record each exact command, exercised path or input, before result, after result, broader check outcome, failure, and limitation.
5. Read the title and description last. Record every implementation, scope, testing, issue-link, checklist, artifact, and disclosure claim. This order keeps the description from substituting for what the diff and output show.

## Evidence gathering

1. Build a fidelity matrix. Put every planned change and deviation in one column and its matching diff hunk in the other. Add each diff hunk that lacks a plan or deviation source, each planned item missing from the diff, and each description claim that conflicts with either side.
2. Build a behavior-evidence matrix. For each reproduction trigger and each failure mode still claimed by the current plan, pair the exact before observable with the after observable from the same affected path. Record controls separately so they cannot substitute for the trigger.
3. Build a repository-check record. List every applicable required executable or planned verification check beside its command or check name, its explicit outcome, and any disclosed reason for a non-passing outcome. Accept concise outcome summaries; mark an absent executable check or unstated outcome as missing. Route checklist-only standards to the standards record.
4. Build a reviewability inventory from the diff and commits. List unrelated changes, debug statements, dead or commented-out attempts, formatting-only churn, temporary files, generated output, and investigation scaffolding. Record `none found` only after every hunk has a purpose in the fidelity matrix.
5. Build a standards record. Pair each applicable template section, issue-linking rule, checklist item, required documentation artifact, disclosure policy, and maintainer request with the description text or diff artifact that satisfies it.
6. In live mode, apply `voice-guide.md` to the title and description and record violations separately from rubric evidence.

## Check execution

1. Grade checks in rubric order using the matching record and the evidence-guide locations. Use the current plan after recorded deviations, not an obsolete promise, as the implementation boundary.
2. Apply each pass condition to the artifact pair it names. Give `pass` only when the record satisfies the complete condition, `fail` when present evidence contradicts it, and `unclear` when required evidence is absent or ambiguous.
3. Write one decisive fact or short quote for every grade. For a compound check, cite the first unsatisfied requirement in the check's pass-condition order.
4. Reuse an already recorded fact when another check needs it, but apply that check's distinct rule. Re-read source material only to resolve a conflict in the records.
5. Treat an honestly disclosed shortfall as evidence, not an automatic failure. It passes only when the current plan and description narrow the claim consistently and the remaining claimed behavior is proved.

## Verdict assembly

1. Confirm that every rubric row has exactly one `pass`, `fail`, or `unclear` grade and one evidence line.
2. Apply the rubric verdict rule without averaging. The first failed or unclear required check in rubric order is the primary reason for `reject`; retain all other failed checks in the summary and JSON.
3. In live mode, append any voice-guide findings to the readable summary without creating extra JSON checks unless a rubric row also failed on that fact.
4. Emit the readable per-check summary, then the contract JSON block. Preserve rubric check names exactly, use the PR URL or bundle id as `item`, and place nothing after the closing fence.

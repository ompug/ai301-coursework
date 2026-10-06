# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. In live mode, read `scope.md` and refuse an out-of-scope issue. Then read the issue body, full thread, contribution guide, and any instructions that explicitly govern issue comments. In eval mode, read the issue context, thread highlights, and repo-facts block only.
2. Read the reproduction evidence before the candidate plan. Record the exact trigger, observed failure, expected result, controls, environment differences, and limits. This prevents a confident plan from replacing the evidence with its own story.
3. Read the candidate plan and record its diagnosis, chosen change, exclusions, named files or components, test commands, expected results, risks, unknowns, and deviations.
4. Read the candidate plan comment last. Record what it promises publicly, how it engages the thread, and whether it differs materially from the full plan.

## Evidence gathering

1. Build a diagnosis record with the plan's cause beside the reproduction's trigger, failing artifact, controls, and limitations. Mark contradictions rather than resolving them in the plan's favor.
2. Build a scope record with every in-scope change, explicit exclusion, named file or subsystem, and extra change implied by the approach. Compare it with the issue's requested behavior.
3. Build an execution record with the selected implementation layer, concrete work steps, ordering dependencies, and every decision deferred to build time.
4. Build a test record pairing each proposed check with the reproduced trigger and the exact observable that must change after the fix.
5. Build a communication record from the plan comment, explicit maintainer requests, competing or prior work mentioned in the thread, and only the repository rules that apply to this comment type.
6. Record unknowns and deviations from the plan itself. Do not infer unstated certainty or invent a risk on the author's behalf.

## Check execution

1. Grade the checks in rubric order using only the evidence record named by that row and the locations defined in the evidence guide.
2. For each check, apply its pass condition literally and write one decisive fact or short quote. Do not award a pass for polish, length, headings, or plausible intent.
3. Grade `fail` when present evidence violates the pass condition. Grade `unclear` when required evidence is missing or too ambiguous to apply the condition. Do not repair a package with outside assumptions.
4. Reuse a gathered fact across checks when relevant, but apply each check's distinct outcome rule. Re-read a source only when two recorded facts conflict.
5. In live mode, report voice-guide violations separately. They affect the verdict only if a rubric check also covers the same failure.

## Verdict assembly

1. Confirm that every rubric row has exactly one grade and one evidence statement.
2. Apply the rubric verdict rule without averaging: any required `fail` or `unclear` produces `reject`; otherwise produce `accept`.
3. In the readable summary, name every failed or unclear check and the fact that decided it. For an accept, give one compact line per check.
4. Emit the required fenced JSON last. Preserve the rubric's check names exactly and quote or paraphrase the decisive package fact in each `evidence` field.

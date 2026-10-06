# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis follows the evidence | Read the candidate plan's stated cause against the issue context and the reproduction block's observed output, controls, and stated limitations. In live mode, use the student's quoted evidence and posted reproduction comment. | Pass when the chosen cause explains every material observation and is either directly supported or is a bounded causal hypothesis that the planned tracing or tests can falsify. The reproduction need not prove the internal mechanism before planning. Fail when a control or artifact rules the cause out, the plan ignores a decisive signal, or an adjacent failure is treated as the target. | required |
| Scope is bounded to the issue | Read the plan's in-scope change, exclusions, named files or areas, and risks against the issue request and repository facts. | Pass when the work is one coherent change needed to resolve the issue, with adjacent defects and opportunistic redesigns left out. Necessary focused tests and small supporting edits pass. Unrequested migrations, broad refactors, new options, or unrelated cleanup fail. | required |
| Approach targets the cause | Compare the chosen code or documentation change with the grounded diagnosis and with any direction or prior art in the thread. | Pass when the proposed change acts on the supported or still-falsifiable causal mechanism and the test plan checks the issue behavior. Do not call a targeted boundary fix symptom-hiding merely because related cached state is deferred. If the thread establishes a direction, the approach must follow it or give an evidence-based reason for a different path. | required |
| Plan is executable | Read the plan's named files or components, selected approach, work order, and unresolved choices. | Pass when a stranger familiar with the repository can begin the change and complete its material steps without choosing the behavior or scope for the author. Exact symbols, compatibility confirmation, and ownership between two named adjacent components may be resolved with a stated trace during the build when the boundary and intended behavior are fixed. Investigate-first plans with no chosen mechanism, unknown subsystems, or mutually exclusive approaches left for build time fail. | required |
| Test plan proves the change | Read the proposed tests and expected results against the issue's expected behavior and the reproduction steps and artifacts. | Pass when the plan names a repeatable check through the affected code path and an observable post-change result that distinguishes success from the reproduced failure. A focused regression test plus relevant broader checks passes. A generic test-suite run, subjective outcome, or check unrelated to the reproduced trigger fails. | required |
| Comment follows thread and conventions | Read the candidate plan comment against thread highlights, repository contribution rules, applicable templates, and any stated AI-use policy in the repo facts. | Pass when the comment states the student's own diagnosis, bounded approach, and verification intent; responds to explicit maintainer direction or competing work when present; and follows every convention that applies to an issue plan comment. In eval mode, when repo facts require disclosure of all AI use, the candidate comment must include that disclosure; silence is not evidence that the frozen candidate package was unassisted. In live mode, require disclosure only when the repository explicitly applies that rule to the student's comment. Boilerplate, piggybacking, or ignored maintainer direction fails. | required |

## Verdict rule

Accept only when every required check passes. Reject when any required
check fails or is unclear. Preferred checks, if added later, may inform
the summary but never change the verdict.

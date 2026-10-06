# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Claim is specific and bounded | Read the claim comment against the issue title, body, and thread. Identify the behavior named, the investigation promised, and any assertions about work already completed. | Pass when the claim identifies this issue's concrete behavior or failing surface, states a relevant next investigation step, and promises a follow-up report without promising a fix, an outcome, or a completion date. A claim may be brief, but boilerplate that could be pasted on an unrelated issue fails. If the claim says reproduction already happened, that assertion must be supported by the accompanying repro report; in claim-only mode, a future-tense investigation is sufficient. | required |
| Environment identifies the tested state | Read the repro report's environment record and code-state statements against the issue's targeted platform, versions, and repository facts. | Pass when the report names the code revision or otherwise unambiguously identifies the tested code state, the operating system, and every runtime or dependency version material to the reported behavior. If the environment differs from the issue's stated target, the difference and its effect on what the run can prove must be stated. Omitted facts that prevent a stranger from recreating the relevant execution state fail. | required |
| Steps are independently rerunnable | Read the repro report's setup and execution steps, including commands, code snippets, starting directory, required services or fixtures, and the trigger for the behavior. Read them together with inputs or scripts fully stated in the issue context when the report explicitly incorporates those by reference. | Pass when a stranger can use the package's issue context plus the report to recreate the material trigger and run the supplied command without inventing a behavior-changing input or setup action. The report need not duplicate issue text it clearly references, and incidental fixture contents need not be listed when the relevant malformed or triggering element is identified. An alternative path passes when its commands are complete and the report explains the boundary it does and does not test. Private files, unavailable configuration, or an omitted material trigger fail. | required |
| Artifacts show the issue behavior | Read the report's output, log, traceback, screenshot description, or other captured artifact against the issue's stated observed and expected behavior. Include controls when the artifact could also be explained by setup failure or an adjacent defect. | For a reproduced conclusion, pass only when the artifact directly exhibits the issue's behavior. For an honest cannot-reproduce conclusion, pass when the artifact records a relevant attempt and the report identifies any unestablished trigger, environment difference, or likely reason the attempt missed; it need not prove the issue absent. A confident statement without captured evidence fails. Evidence of a different error, dependency outage, or neighboring bug fails when it is presented as reproduction of the target. | required |
| Conclusion matches the evidence | Compare the report's reproduced, cannot-reproduce, or conditional conclusion with its artifacts, controls, limitations, and deviations. Read the conclusion as a whole, including qualifications that immediately narrow its headline. | Pass when the conclusion says what the evidence establishes, identifies material limitations or substitutions, and does not claim the issue is false merely because an attempt missed. An evidenced cannot-reproduce passes even when the report explains that the exact trigger may not have been reached. A reproduction claim based on the wrong target, a hidden deviation, or evidence supporting only an adjacent behavior fails. | required |
| Repository conventions are followed | Read the repo-facts block or, in live mode, the repository's contribution guide, applicable comment instructions, and stated AI-assistance policy. Distinguish a template for filing a new issue or pull request from rules that explicitly govern follow-up issue comments. | Pass when the comments satisfy every convention that applies to them, including a required AI-use disclosure when the repository states one. If no disclosure policy applies to issue comments, absence of disclosure does not fail. An existing issue's filing template does not require a follow-up comment to repeat inputs already present in the issue unless the repository explicitly says it does. The comments must remain the student's own report rather than piggybacking on another contributor's evidence. | required |

## Verdict rule

Accept only when every required check passes. Reject when any required
check fails or is unclear. In claim-only live mode, leave checks whose
evidence requires the repro report out of the verdict as directed by
`SKILL.md`; every applicable claim or conventions check must still pass.

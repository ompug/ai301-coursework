# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

In an eval bundle, start with the issue context and repo-facts block for
the affected versions or platform, then read the repro report's
environment and setup text. In live mode, use the issue, dependency or
project manifests, setup documentation, the checked-out revision, and
the student's draft. Do not infer a version from what is merely
installed elsewhere on the machine.

Good evidence identifies the tested code state, operating system, and
the runtime and dependency versions that can change the observed path.
A report using a substitute database, operating system, or invocation
states that difference and limits its conclusion to what the substitute
actually proves. Extra unrelated package versions are not required.

## Steps

In an eval bundle, read setup, commands, snippets, inputs, and the final
trigger across the issue context and candidate repro report. A report
may explicitly incorporate a script or input that is fully stated in
the issue instead of copying it. The repo-facts block can establish
documented prerequisites but cannot silently fill a private setup gap.
In live mode, read the draft as a future reader will see it alongside
the issue page; files only in the student's working directory are not
part of the posted package unless their contents or a durable link are
included.

Good steps identify the relevant starting state and provide enough
commands and behavior-changing input to reach the observation. They do
not need to repeat issue text they clearly reference or list incidental
fixture contents. A stranger must not have to invent a material trigger,
private configuration, or unavailable file. An alternative path states
the boundary it does and does not exercise.

## Behavior shown

The issue context defines the target: compare its expected and observed
behavior with output excerpts, logs, tracebacks, response bodies,
screenshots, or other artifacts in the repro report. Repo facts and the
claim provide context, but neither substitutes for observed evidence.
In live mode, gather issue-side expectations from the issue thread and
read candidate-side proof only from the draft.

Good evidence for a reproduction contains the value, error, state
transition, or visible result that distinguishes the target bug. When a
setup failure or nearby bug could produce the same top-level symptom,
the report includes a control or narrower artifact. For a
cannot-reproduce result, captured output from the relevant attempt is
evidence when the report names any missing trigger or environment
difference; the report need not prove the issue absent.

## Honesty

Compare every claim in the conclusion with the report's artifacts,
controls, environment, and stated deviations. Also compare a claim
comment's past-tense assertions with the accompanying report. In live
claim-only mode, future-tense plans do not need reproduction evidence;
unsupported assertions that the bug was already reproduced do.

Good language distinguishes observation from inference, names material
limitations, and keeps neighboring failures separate. It may conclude
reproduced, cannot reproduce, or reproduced only under stated
conditions. A cannot-reproduce result can still explain that the exact
trigger may not have been reached; that candor is a strength, not a
contradiction. It does not convert a unit-level result into an untested
end-to-end claim or present another contributor's evidence as the
student's run.

## Comms

In an eval bundle, use the issue context for specificity and the
repo-facts block for contribution rules and AI-assistance disclosure
requirements. Note whether a template governs filing the issue or the
follow-up comments being graded. In live mode, inspect the issue,
contribution guide, and any explicit comment instructions. A stated
policy is evidence; convention must not be invented from another
repository or transferred from a PR template to issue comments without
support.

A good claim names the issue's actual failing surface, says what the
student will investigate next, promises the report, and avoids promises
of a fix, result, or date. Good comments comply with every stated
repository rule. If disclosure is required, it says what AI assisted
with and does not imply the AI ran commands the student did not run. If
no policy requires disclosure, its absence is neutral. Each report uses
the student's own environment and words even when classmates have
posted on the same issue.

---
name: pr-precheck
description: Grade a PR package against its issue, accepted plan, diff, test evidence, and repository rules to decide whether it is ready to submit. Use for a live draft PR before opening it or for a frozen eval package.
---

# pr-precheck: rubric-driven PR grading

## The question

Grade exactly one PR package and answer one question: is this pull request ready to submit? A package contains the candidate title, description, commits, diff, and test evidence. Read those artifacts against the issue, the accepted plan including recorded deviations, and the repository's stated requirements. Do not review unrelated work or propose a different implementation.

## Inputs and modes

Choose the mode from the supplied input.

- **Live mode:** Read the issue URL named by the request, `plan.md` including `## Deviations`, the committed branch diff from `git diff main...HEAD`, the title and description in `pr_draft.md`, and the captured output in `test_evidence.md`. Read the full issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md` from the scoped repository. The three-dot diff is the entire submitted implementation; uncommitted code is not part of it. For a house-chain submission, use the assigned house plan and repro pack in place of the student's own posted plan and report.
- **Eval mode:** Treat the supplied package bundle as the whole world. Use only its issue context, thread highlights, repo facts, plan context, candidate PR, diff, commits, and test evidence. Fetch nothing and read no outside files. Grade every rubric check and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` first. Apply its repository boundary and house rules. Refuse to grade a PR targeting any other repository. If the `Repo:` line still contains its bracketed placeholder, stop without grading and tell the student to fill it with the section's Path Review repository. Never infer the missing value. Eval mode ignores `scope.md`.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and check both the PR title and description against it. Report each broken rule by name with the text that breaks it. Voice findings change the verdict only when a rubric check covers the same problem. Eval mode ignores `voice-guide.md`.

## Component reads

Read `rubric.md` for the check names, evidence sources, pass conditions, weights, and verdict rule. Use `references/evidence-guide.md` to locate each evidence family in the selected mode. Execute `procedure.md` as written for read order, evidence pairing, grading, and verdict assembly.

If `rubric.md` has no completed checks or `procedure.md` has no authored steps, refuse to grade and identify the empty component. If the procedure is silent on a necessary operation, report that gap instead of inventing a step.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject` means hold. Give a compact readable per-check summary, then end with the following valid fenced JSON block. Nothing follows it. Preserve the schema and key order exactly.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Grade each check from a named fact or quote. "Looks fine" is not evidence.
- Grade the artifacts and their agreement, not their length or polish.
- Apply each rubric condition literally. When a result feels wrong, preserve the result and fix the rubric later.
- Follow the procedure literally. Report procedural gaps instead of filling them with unstated judgment.
- Apply the rubric's `unclear` rule. If it is silent, treat `unclear` as a failure because the package does not verify the claim.

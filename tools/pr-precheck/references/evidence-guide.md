# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives:** In eval mode, read the plan-context scope, files, exclusions, and deviation notes beside the candidate diff, commits, and description. In live mode, read `plan.md` including `## Deviations`, `git diff main...HEAD`, and the change and limitation claims in `pr_draft.md`.

**What good looks like:** Every material hunk implements a planned item or a reasoned recorded deviation, every current plan promise appears in the diff, and the description matches both. A smaller outcome is faithful when the deviation and description state it plainly. Silent additions, silent omissions, and claims that exceed the diff are drift.

## Test evidence (harness category: not-tested)

**Where it lives:** In eval mode, compare the plan-context reproduction and test plan with the candidate test-evidence section and testing claims. In live mode, compare the posted reproduction and `plan.md` test plan with `test_evidence.md` and the Testing section of `pr_draft.md`.

**What good looks like:** The same trigger and affected path show the failing before state and a distinguishable successful after state for every distinct behavior still claimed. A representative test can cover equivalent callers governed by the same changed mechanism, while a separate failure mode needs its own proof. Controls do not replace the trigger. Required repository checks appear with unambiguous outcomes; a concise summary is enough, while an expected empty suite or disclosed unrelated failure remains visible instead of being reported as passing.

## Diff quality (harness category: unreviewable)

**Where it lives:** In eval mode, inspect every hunk of the candidate unified diff and use the commit list as supporting context. In live mode, inspect all of `git diff main...HEAD` and the commits in that range. Do not infer cleanliness from the description.

**What good looks like:** Each hunk belongs to the bounded fix, focused tests, or required documentation, and the intended behavior change is easy to locate. Debug output, dead helpers, commented-out attempts, temporary instrumentation, unrelated cleanup, generated debris, and broad formatting churn make the diff unreviewable.

## Standards and comms (harness category: standards-wall)

**Where it lives:** In eval mode, pair the repo-facts PR-template asks, contribution rules, disclosure policy, and thread direction with the candidate title, description, checklist, and diff artifacts. In live mode, pair `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the issue thread with `pr_draft.md` and `git diff main...HEAD`.

**What good looks like:** The title names the change, the issue is linked in the required form, every applicable section and checklist item is answered truthfully, required tests or documentation artifacts exist, applicable disclosure language is present, and explicit maintainer direction is addressed. A non-executable hygiene checklist item may be satisfied by an attestation when the diff does not contradict it. Ordinary competing work requires acknowledgment only when the repository or a maintainer says so. In eval mode, thread highlights are excerpts: their silence does not disprove an accepted plan named by the plan-context block. Unrun CI may remain unchecked when stated honestly. Boilerplate, silent omissions, or a description that asserts compliance without the required artifact fails.

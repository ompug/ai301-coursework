# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repository is writable | In the repo-facts block, read the `archived` value on the repo line. In live mode, read the repository banner and archived state. | Pass only when the repository is not archived and is still writable. | required |
| Recent human activity | Compare the capture date with `last push to any branch` and the dates and authors under `last 5 default-branch commits` in the repo-facts block. In live mode, use the repository's last-push date and five newest default-branch commits. | Pass when a branch was pushed or a non-bot author committed to the default branch within the previous 365 days. A username ending in `[bot]` is not human activity unless the commit explicitly merged a human-authored pull request. | required |
| Bounded contribution | Read the issue title, author association, labels, and body, then the comment thread for scope changes or maintainer decisions. Also read issue age and linked-PR history in the repo-facts block when the thread shows repeated attempts. | Pass when the issue asks for one observable fix, one documentation change, or one tightly related acceptance set that a newcomer can verify. Treat edits to several named documentation pages as one bounded change when they all move or update the same guidance and the issue states what belongs in each page. A short issue filed by a maintainer and labeled `good first issue` passes when its title names one UI behavior or feature area, even if its body gives several examples followed by `etc.` Multiple possible causes or implementation suggestions do not fail an otherwise single defect. Fail an umbrella or tracking issue, a list of unrelated changes intended for separate contributors, a pure usage question, a change explicitly requiring core internals, or work whose product/design behavior remains unsettled. Also fail when years of discussion and at least two closed unmerged linked pull requests show repeated abandoned attempts without a later maintainer comment confirming a current specification. | required |
| Available to start | Read `this issue: assignees` and `linked PRs` in the repo-facts block, plus claim and work-status statements in the comment thread. Compare claim dates with the bundle capture date. In live Path Review mode, apply the claim exception in `scope.md`. | Pass when there is no assignee, no open linked pull request, and no contributor statement within the previous 180 days that work is in progress. Closed or merged pull requests are not active claims. Claims older than 180 days are stale unless a later comment says the work continues. A later maintainer invitation to new contributors also clears a stale claim. In Path Review live mode, student claim comments are ignored as required by the house rule. | required |
| AI-assisted contribution allowed | Read the contribution-policy line in the repo-facts block. In live mode, read `CONTRIBUTING.md`, any contributor docs it links, dedicated AI-policy files, and pull-request templates. | Pass when the policy is silent about AI or permits AI-assisted work, including policies that require disclosure, testing, review, or personal understanding. Fail only when the repository bans AI-generated or AI-assisted code or documentation. | required |
| Newcomer guidance | Read the issue labels, body, and maintainer comments for a `good first issue` label, named files, reproduction steps, acceptance criteria, or a maintainer diagnosis. | Pass when at least one of those concrete starting signals is present. Otherwise mark fail. This check ranks accepted issues but never changes the verdict. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only when every required check passes. Reject when any required check fails or is unclear. The preferred check never changes the verdict; use it only to rank issues that pass all required checks.

# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

## Grading: issue #61 — "Health check DB probe passes a raw SQL string, which fails under SQLAlchemy 2.x"

In scope: the issue is in `codepath/pathreview-ai301-fa26-s3`, the only repo `scope.md` permits. Live mode, single candidate, graded against today (2026-09-23).

| Check | Grade | Evidence |
|---|---|---|
| Repository is writable | **pass** | `isArchived: false`, `visibility: PUBLIC` — no archived banner. |
| Recent human activity | **pass** | Last push 2026-09-16 (7 days ago); all 5 newest `main` commits authored by `Aburke225`, not a `[bot]`. |
| Bounded contribution | **pass** | One defect at one call site: `await db.execute("SELECT 1")` in `api/routes/health.py`, which I confirmed still reads exactly that on `main`. Not an umbrella, not a question, no unsettled design. |
| Available to start | **pass** | `assignees: []`, `closedByPullRequestsReferences: []`, zero comments, no PR referencing #61. Timeline holds only four `labeled` events from the author. |
| AI-assisted contribution allowed | **pass** | `docs/CONTRIBUTING.md` (the only contributing doc; no `AI_POLICY.md`, no PR template) says nothing about AI, LLMs, or generated code — silence passes. |
| Newcomer guidance *(preferred)* | **pass** | Labeled `good first issue` and `tier-1`; body names the file and gives repro steps plus the exact `ArgumentError` text. |

**Verdict: accept** — every required check passes.

Two things worth knowing before you start. The fit is strong for your profile: the fix is a one-line change (wrap in `sqlalchemy.text()`), and since there's no health test file anywhere in the tree, the real work is the focused regression test you said you wanted practice writing — a bounded one-session task with a direct reproduction path. Second, `docs/CONTRIBUTING.md` notes that seeded bugs may carry a `@pytest.mark.xfail(strict=True)` marker that you must *remove* as part of the fix; I found no such marker tied to #61, but check for one when you open the code. The house rule in `scope.md` also means you should claim the issue by commenting regardless of what classmates have posted.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
  "checks": [
    {"name": "Repository is writable", "grade": "pass",
     "evidence": "gh repo view reports isArchived: false, visibility PUBLIC; no archived banner."},
    {"name": "Recent human activity", "grade": "pass",
     "evidence": "Last push 2026-09-16 (7 days before today); all 5 newest main commits authored by human user Aburke225."},
    {"name": "Bounded contribution", "grade": "pass",
     "evidence": "One observable defect at one call site: `await db.execute(\"SELECT 1\")` in api/routes/health.py, confirmed present on main."},
    {"name": "Available to start", "grade": "pass",
     "evidence": "assignees: [], closedByPullRequestsReferences: [], zero comments, no PR referencing #61; timeline shows only the author's four labeled events."},
    {"name": "AI-assisted contribution allowed", "grade": "pass",
     "evidence": "docs/CONTRIBUTING.md is the only policy doc and is silent on AI/LLM/generated code; no AI_POLICY.md or PR template exists."},
    {"name": "Newcomer guidance", "grade": "pass",
     "evidence": "Labels include `good first issue` and `tier-1`; body names api/routes/health.py plus repro steps and the exact ArgumentError message."}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run: `agreement: 16/20 scored items  (bar: 18/20: below the bar)`
2. Targeted rerun of issue-01, issue-04, issue-09, and issue-15: `agreement: 2/4 scored items`
3. Targeted rerun of the same four issues after revising the scope check: `agreement: 4/4 scored items`
4. Final full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

For `issue-15`, my rubric decided `reject`, which matched the gold label `reject`. The issue had no current assignee and its linked pull requests were closed, so it passed my availability check. It failed `Bounded contribution`, though. The evidence showed two closed, unmerged linked pull requests and a claim-and-unassign cycle involving several contributors from 2021 through 2024. There was no later maintainer comment confirming a current specification. Under the current check, that history indicates repeated abandoned attempts and unsettled work rather than a bounded first contribution.

**Check rationale**

My `Available to start` check currently says: “Pass when there is no assignee, no open linked pull request, and no contributor statement within the previous 180 days that work is in progress. Closed or merged pull requests are not active claims. Claims older than 180 days are stale unless a later comment says the work continues. A later maintainer invitation to new contributors also clears a stale claim. In Path Review live mode, student claim comments are ignored as required by the house rule.” I used a 180-day boundary so an old request to work on an issue does not block it forever, while a recent claim still protects work that is likely active. Assignees and open linked pull requests remain hard failures because they are stronger evidence than a comment alone.

**Trade-offs**

The 180-day rule changed `issue-09` from reject to accept in my targeted rerun. Its only claim was from January 2022, about four and a half years before the bundle capture date, and its linked pull request was closed. The trade-off is that this check can accept an issue where someone is still working privately after six months without posting an update. I accept that risk because the public evidence no longer shows active work, and maintainers can clear or renew the signal in the thread.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This issue fits my interest in Python API behavior and gives me a focused chance to work with SQLAlchemy and pytest. The failure is at one call site, and the issue provides the exact exception, so I expect to reproduce, fix, and test it within one course work session.
2. The verdict correctly identified that the repository is active, the issue is unassigned, the work is bounded, and the contribution policy permits this workflow. Outside the rubric, I weighed the learning value of adding a regression test against the setup time for the API and database. I chose it because the testing work is useful even though the code correction itself may be small.
3. Claiming it should be straightforward because the issue has no assignee, comments, or linked pull request. I still need to follow the Unit 2 claim format and post my reproduction evidence before proposing the fix. The Path Review house rule means another student's later claim would not block my coursework contribution.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

# Unit 3 - Plan and Build

## Posted upstream

**GitHub username**

ompug

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-6008590631

Plan for issue #61, based on [my reproduction report on this thread](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-6007985078) at `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.

The failing call is the PostgreSQL probe in `api/routes/health.py`: it passes the bare string `"SELECT 1"` to `db.execute`. My run captured `sqlalchemy.exc.ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')`. In the same async session, the control `text("SELECT 1")` returned `scalar=1`, so the evidence points to SQLAlchemy statement coercion rather than an unreachable database.

I will import `text`, change the probe to `await db.execute(text("SELECT 1"))`, and add a focused test in `tests/unit/test_health.py`. The test will assert that the real handler passes a `TextClause` containing `SELECT 1` and records PostgreSQL as healthy when the database call succeeds. I will also re-run the Unit 2 route-handler reproduction and expect `postgres_health_check_passed` with `dependencies.postgres == "healthy"`.

This stays limited to issue #61. Redis issue #62, exception-policy changes, configuration, `pyproject.toml`, and broader health-route cleanup are out of scope. Because my reproduction used SQLite, I am limiting the claim to the statement-coercion failure and the handler state it produces; I am not claiming a PostgreSQL end-to-end run.

I also saw the open work in PR #96 and the broader PR #82. Under the Path Review house rules I am still building my own branch from my own reproduction; this plan remains limited to the #61 probe and its regression test rather than adopting the Redis changes in #82.

## Your branch

**Branch**

`fix/61-health-check-sql-text`

**Evidence**

Before the change, from commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`:

```bash
PYTHONPATH=. .venv-repro/bin/python /home/ompug/Documents/Codex/2026-10-05/sk-or-v1-*/work/verify_61_unit3.py
```

```text
SQLAlchemy=2.1.3
database_url=sqlite+aiosqlite:///./ompug-unit3-61.db

[A] raw-string control
sqlalchemy.exc.ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')

[B] declared-text control
scalar=1

[C] real health_check handler
2026-10-05 23:15:15 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
2026-10-05 23:15:15 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'"
2026-10-05 23:15:15 [debug    ] vector_db_health_check_passed
status_code=503
status=unhealthy
dependencies={'postgres': 'unhealthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}
```

The regression test also failed before the source edit:

```bash
.venv/bin/pytest tests/unit/test_health.py -v
```

```text
tests/unit/test_health.py::test_health_check_uses_text_clause_for_postgres_probe FAILED
E       AssertionError: assert False
E        +  where False = isinstance('SELECT 1', TextClause)
======================== 1 failed, 2 warnings in 3.78s =========================
```

After commit `cb90866`, the same reproduction command produced:

```bash
PYTHONPATH=. .venv-repro/bin/python /home/ompug/Documents/Codex/2026-10-05/sk-or-v1-*/work/verify_61_unit3.py
```

```text
SQLAlchemy=2.1.3
database_url=sqlite+aiosqlite:///./ompug-unit3-61.db

[A] raw-string control
sqlalchemy.exc.ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')

[B] declared-text control
scalar=1

[C] real health_check handler
2026-10-05 23:18:32 [debug    ] postgres_health_check_passed
2026-10-05 23:18:32 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'"
2026-10-05 23:18:32 [debug    ] vector_db_health_check_passed
status_code=503
status=unhealthy
dependencies={'postgres': 'healthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}
```

The focused regression test then passed. The broader checks also passed:

```text
.venv/bin/pytest tests/unit/test_health.py -v
======================== 1 passed, 2 warnings in 2.18s =========================

.venv/bin/pytest tests/unit -v -m unit
================= 376 passed, 53 xfailed, 3 warnings in 18.12s =================

.venv/bin/ruff check api/routes/health.py tests/unit/test_health.py
All checks passed!

.venv/bin/black --check api/routes/health.py tests/unit/test_health.py
All done!
2 files would be left unchanged.

.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```

The raw-string control still raises and the declared-text control still returns `1`. The handler now passes the PostgreSQL probe. Its 503 comes from the separate Redis configuration defect in issue #62.

## Eval iterations

**Run history**

1. Complete run: 17/20.
2. Targeted run for `pkg-03`, `pkg-14`, and `pkg-20`, with `pkg-10` and `pkg-04` as canaries: 4/5.
3. Targeted run for `pkg-14` with `pkg-03`, `pkg-01`, `pkg-07`, `pkg-17`, and `pkg-18`: 6/6.
4. Confirming complete run: 20/20. Category results were clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, and wrong-cause 4/4.

**Package analysis**

For `pkg-20`, the initial rubric verdict was `accept` while the gold verdict was `reject`. The package's repository facts explicitly required disclosure of all AI use, but the candidate comment omitted it. My original convention check treated the omission as unknown and accepted the comment. I revised that check so eval packages with an explicit all-use policy require the frozen candidate comment to contain the disclosure. The revised rubric returned `reject`, matching the gold label.

**Check rationale**

The submitted `Plan is executable` pass condition reads:

> Pass when a stranger familiar with the repository can begin the change and complete its material steps without choosing the behavior or scope for the author. Exact symbols, compatibility confirmation, and ownership between two named adjacent components may be resolved with a stated trace during the build when the boundary and intended behavior are fixed. Investigate-first plans with no chosen mechanism, unknown subsystems, or mutually exclusive approaches left for build time fail.

The first version rejected plans that left any implementation detail to tracing. That was too strict for `pkg-03` and `pkg-14`, where the behavior and boundary were already fixed and the trace only resolved an exact symbol or handoff point. The revised wording permits that bounded trace but still rejects plans that defer the mechanism or scope.

**Trade-offs**

Loosening the executability check could let a vague investigate-first plan pass, so the final sentence preserves that rejection boundary. After the change, `pkg-14` moved to the correct `accept`; `pkg-17` and `pkg-18` remained `reject` as unbuildable canaries. Tightening the convention check changed `pkg-20` to `reject`, while `pkg-04` stayed `reject` as the other thread-convention canary.

# Plan for issue #61

## Diagnosis

The PostgreSQL health probe in `api/routes/health.py` passes the bare string `"SELECT 1"` to `AsyncSession.execute`. SQLAlchemy 2.x rejects that argument during statement coercion, before a database dialect executes it. The handler catches the resulting `ArgumentError` and records PostgreSQL as unhealthy.

My reproduction at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` captured the failure:

```text
sqlalchemy.exc.ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')
```

The control used the same live async session and returned:

```text
[B] control in the same session using text()
scalar=1
```

The real handler then returned 503 with:

```text
dependencies={'postgres': 'unhealthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}
```

The control shows that the session can execute the statement when it is explicitly declared as textual SQL. This reproduction used SQLite rather than PostgreSQL, so it establishes the SQLAlchemy statement-coercion failure and the handler state it causes, not PostgreSQL-specific behavior.

## Scope

In scope:

- Import `text` from SQLAlchemy in `api/routes/health.py` and pass `text("SELECT 1")` to `db.execute`.
- Add a focused unit regression test in `tests/unit/test_health.py` that verifies the real handler passes a `TextClause` containing `SELECT 1` and records PostgreSQL as healthy when the database call succeeds.

Out of scope:

- Redis issue #62 and the missing `settings.redis_host` and `settings.redis_port` attributes.
- Changes to the broad dependency exception handling.
- Configuration, response-schema, database-engine, or wider health-route refactoring.
- Changes to `pyproject.toml`; its current health-module suppressions also cover the separate Redis defect.

## Files

- `api/routes/health.py`
- `tests/unit/test_health.py`

`plan.md`, `comment.md`, reproduction scripts, virtual environments, and SQLite files remain uncommitted.

## Approach

1. Add a regression test around `health_check` using an `AsyncMock` database session.
2. Patch `core.config.settings` with test values and patch `redis.Redis` to make the unrelated Redis probe succeed. This isolates the PostgreSQL behavior without changing production configuration.
3. Assert that the database session receives one `TextClause`, that `str(statement)` is `SELECT 1`, and that the returned dependency state marks PostgreSQL healthy.
4. Run the test before the source change and retain its expected failure when the handler passes a plain string.
5. Import `text` from `sqlalchemy` in `api/routes/health.py` and change only the PostgreSQL probe to `await db.execute(text("SELECT 1"))`.
6. Run the focused test, the full unit suite, lint, formatting check, type checking, and the before-and-after reproduction.

## Test plan

### Regression test

Run:

```bash
.venv/bin/pytest tests/unit/test_health.py -v
```

Before the fix, the assertion that the database argument is a `TextClause` must fail because the handler passes a string. After the fix, the test must pass, `str(statement)` must equal `SELECT 1`, and the otherwise healthy mocked handler response must contain `dependencies.postgres == "healthy"`.

### Unit and static checks

Run:

```bash
.venv/bin/pytest tests/unit -v -m unit
.venv/bin/ruff check api/routes/health.py tests/unit/test_health.py
.venv/bin/black --check api/routes/health.py tests/unit/test_health.py
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
```

All commands must exit successfully. The new files must introduce no lint, formatting, or type errors.

### Reproduction after the fix

Re-run a neutral verification script with the same SQLite async session used for the Unit 2 reproduction. It will execute the raw-string control, the `text()` control, and the real handler.

Expected after the fix:

- The direct raw-string control still raises `ArgumentError`; that is SQLAlchemy 2.x behavior.
- The direct `text("SELECT 1")` control still returns scalar `1`.
- The real handler logs `postgres_health_check_passed` and reports `dependencies.postgres` as `healthy`.
- The handler may still return 503 because Redis issue #62 remains out of scope. That result must not be reported as a failure of the PostgreSQL fix.

## Risks and unknowns

- The reproduction uses SQLite, not PostgreSQL. Because the failure occurs before dialect execution and the control succeeds in the same session, the test is sufficient for this call-shape fix, but it does not replace a future end-to-end PostgreSQL check.
- The route combines three dependency probes. The unit test must isolate Redis so issue #62 cannot make the PostgreSQL regression test pass or fail for the wrong reason.
- Other raw textual SQL call sites are not part of this issue and will not be changed.

## Deviations

No implementation deviation. The build matched the posted plan: `health.py` wraps the probe in `text()`, the focused test isolates Redis, and the same reproduction now reports PostgreSQL as healthy. The overall handler still returns 503 only because Redis issue #62 is outside this change.

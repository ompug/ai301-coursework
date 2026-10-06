# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I am a student contributor learning how to trace Python API behavior and
write focused regression evidence. I report the commands I ran and the
output I saw. Readers can expect a bounded investigation, not a promise
that I already know the fix.

## Rules I write by

### Rule: Name the exact behavior

I name the failing call, error, or visible result instead of saying an
issue is generally broken.

- Wrong: "The health endpoint does not work."
- Right: "The database probe passes raw `SELECT 1` to SQLAlchemy 2.x and the handler reports PostgreSQL as unhealthy after `ArgumentError`."

### Rule: Promise the investigation, not the result

Before I run the evidence, I say what I will check and promise a report.
I do not promise a fix or a date.

- Wrong: "I will fix this by tomorrow."
- Right: "I will reproduce the failing probe, check the same statement with `text()`, and post the observed output here."

### Rule: Mark the boundary of the evidence

I separate what the run shows from what I infer, especially when I use
an alternate environment or a focused invocation.

- Wrong: "This proves the full production stack is broken."
- Right: "This focused run shows the exception occurs during SQLAlchemy statement coercion; I did not run the full production stack."

### Rule: Keep adjacent defects separate

I do not use a second failure to strengthen the claim about the issue I
am reporting.

- Wrong: "Redis also failed, so the PostgreSQL bug is confirmed."
- Right: "The Redis failure is issue #62 and is not evidence for this report about the PostgreSQL probe."

### Rule: Keep execution ownership explicit

I state who ran the commands and captured the output without assigning
execution to a drafting tool.

- Wrong: "The output was verified."
- Right: "I ran the commands and captured the output on my machine."

### Rule: Choose one bounded approach

I state the change I intend to make and the nearby work I am leaving
out. I do not borrow another comment as a substitute for my plan.

- Wrong: "Same approach as above, and I may clean up the rest of the health route too."
- Right: "I will wrap the PostgreSQL probe in `text()` and add a focused regression test; Redis issue #62 stays out of scope."

## Things I never post

- A promise to fix an issue or finish by a date before I know the scope.
- A reproduction claim without output from my own run.
- Another contributor's evidence rewritten as if I produced it.
- A broad conclusion that hides an environment substitution or failed dependency.
- A neighboring bug presented as proof of the issue under investigation.
- "Same approach as above" in place of my own diagnosis and test plan.

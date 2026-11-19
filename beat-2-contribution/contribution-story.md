# Contribution: Pingo Doce locator coverage

**Contribution Number:** [1]
**Student:** Hariom Pughat
**GitHub Username:** ompug
**Issue:** https://github.com/alltheplaces/alltheplaces/issues/7950
**Pull Request:** https://github.com/alltheplaces/alltheplaces/pull/20681

## Toolkit

**Unit 5: Built a scout for Python issues with a pytest check**

On November 18, I searched open Python issues tagged good first issue that mention pytest. The query returned 100 results, with 93 passing the metadata filters, and the scout accepted its top five. The initial query used an unsupported flag, which I changed to --match title,body. I then put API titles first because the original ranking mixed API work with CI setup and GPU documentation.

**Unit 6: Checked claims and installed the voice guard**

I installed the voice guard and confirmed that it stopped a comment command with no readable body. Its first real check passed my claim, which I posted through Claude Code. I wrote a local memory file linking the project's existing agent guidance and the source-specific decisions. Two scout accepts were already implemented, so I checked current source coverage before selecting Pingo Doce.

**Unit 7: Used a manifest and ship-gate before opening the PR**

My manifest runs the project's shared brand tests and pre-commit checks. The mounted filesystem does not preserve executable script permissions, so I invoked the installed tools as Python modules. Ship-gate passed both commands and all five PR checks before I opened the upstream PR.

**Unit 8: Ran the monitor and kept its wait note local**

I installed the monitor and wrote a seven-day nudge threshold using five recent merged contributor PRs. It initially classified an automated Copilot quota notice as feedback, but its drafted move correctly said that no reply was needed. I read the notice and kept the wait note local. I would improve the state classifier to distinguish an automated review failure from a request for changes.

## Why I Chose This Issue

I used Python and good first issue filters on the AI301 Issue Selection site. Several attractive API issues were already claimed or linked to active PRs. I graded HHS/simpler-grants-gov #5277 and MetOffice/CSET #911, then inspected their default branches and found their requested work already present. I selected All the Places #7950 because it was available, had a public source named by a maintainer, and could be verified by comparing the source stores with exported features.

The patched scout accepted Pingo Doce as straightforward. I replaced my strict Bounded contribution check with the supplied scope-feasible rule and separate difficulty notes. The scout helped check policy, activity and claims, but its availability rule missed already-completed work on open issues. I chose a practical Python contribution with observable output, although this project uses shared checks and explicitly excludes individual spider test files. My claim is https://github.com/alltheplaces/alltheplaces/issues/7950#issuecomment-6997361497.

**Toolkit**

- Tool: issue-scout, used for the five-issue scan and direct grading of curated candidates.
- What it produced: saved verdicts and a straightforward difficulty note for #7950.
- Difficulties: open issue status did not prove work was still missing.
- What changed: added source inspection to my selection process and recorded the rubric gap in scan-notes.md.

## Understanding the Issue

**Problem Description**

All the Places did not have a spider for Pingo Doce's Portuguese supermarket locations.

**Expected Behavior**

A registered spider should retrieve the official locator response and export the stores in the project's standard format.

**Current Behavior**

The baseline registry had no Pingo Doce spider even though the official endpoint returned 508 stores.

**Affected Components**

The new locations/spiders/pingo_doce_pt.py uses JSONBlobSpider, DictParser, OpeningHours and the existing item pipelines.

**Toolkit**

- Tool: issue-scout and repository inspection.
- What it produced: confirmation that the source coverage was missing and the issue was available.
- Difficulties: the issue's old comment reported 488 stores, so I needed current evidence.
- What changed: used the current 508-store response for all implementation claims.

## Reproduction Process

**Environment Setup**

Linux, Python 3.11.15 and Scrapy 2.19.0. I cloned All the Places, forked it to ompug/alltheplaces, and installed dependencies with uv sync --group dev. The base commit was 0e6c8ea156378d56f09dfe9972b250b3d6c66737.

**Steps to Reproduce**

1. Check out the base commit and run uv sync --group dev.
2. Run `uv run python -m scrapy list | rg pingo`. Before the implementation it returns no match and exit 1.
3. Search locations/spiders for Pingo Doce and Q1575057. Neither is present.
4. Run `curl -fsSL 'https://www.pingodoce.pt/wp-content/themes/pingodoce/ajax/pd-ajax.php?action=pd_stores_get_stores' | jq '{success,count:(.data.stores|length)}'`.

**Reproduction Evidence**

- **Commit showing reproduction:** none. The missing-spider baseline was captured before adding the new file.
- **Screenshots/logs:** the registry search returned no match, while the official endpoint returned success=true and count=508.
- **My findings:** this is missing coverage requested by a feature issue. The source is usable, and the absent registry entry gives a repeatable before state.

**Toolkit**

- Tool: repro-check, run on the saved claim and reproduction report.
- What it produced: claim-only accept before posting, followed by an accept on all six combined reproduction checks.
- Difficulties: the first claim check refused because the completed scout verdict had not yet been appended to scan-output.md.
- What changed: saved the scout read-out and reran the claim check successfully.

## Solution Approach

**Analysis**

The official response nests stores under data.stores. Each record has a stable ID, name, coordinates, address, postal code, contact, store-page path and weekly schedule. A new spider can use the project's existing JSON parser and normalization pipelines.

**Proposed Solution**

Add one JSONBlobSpider for Portugal, using verified Wikidata Q1575057 and the matching NSI brand Pingo Doce. Map contact, county and district to phone, city and state, join store-page paths to the official domain, and pass weekly schedules to OpeningHours. Dated holiday exceptions are outside this version.

**Implementation Plan**

1. Capture the baseline registry result and source count.
2. Add the spider using locations_key = ["data", "stores"].
3. Verify parsed IDs, coordinates and representative output against the source.
4. Run shared brand tests, pre-commit checks and the cached live crawl.
5. Record evidence, pass ship-gate and open the upstream PR.

During the crawl I found four empty schedule lists and a latitude of zero for store 794. I recorded both in plan.md. Empty schedules produce no hours. Store 794 remains in the output with null geometry.

**Toolkit**

- Tool: plan-check, using plan.md and an unposted plan comment.
- What it produced: accept on all six required checks.
- Difficulties: missing coverage needed a plan around registration and exported data rather than an exception fix.
- What changed: specified the API key path, field mapping, observable result and upstream rule against individual spider tests.

## Testing Strategy

Before the change the registry search returned no Pingo Doce spider. After the change, the following cached crawl completed with item_scraped_count=508:

```bash
uv run python -m scrapy crawl pingo_doce_pt -s HTTPCACHE_ENABLED=True -s HTTPCACHE_EXPIRATION_SECS=604800 -O /tmp/pingo-doce.geojson -L INFO
uv run python -m pytest tests/test_item_attributes.py -q
uv run python -m pre_commit run --files locations/spiders/pingo_doce_pt.py
```

The brand tests passed all 3 tests with one installed anyio deprecation warning. Pre-commit passed the applicable naming, formatting, imports and lint checks. A temporary parsing assertion compared all 508 source IDs and verified 507 nonzero coordinate pairs within Portugal's bounds. The export contains 508 features, 507 with geometry. Store 794 has null geometry because the source reports latitude zero.

Store 451 exports branch 1º de Dezembro, postcode 1200-358, coordinates [-9.140362,38.713742], normalized phone +351 938 875 501, hours Mo-Su 08:00-21:00, and shop=supermarket. Four records have no weekly hours. I followed AGENTS.md's shared-check approach without adding an individual spider test file.

**Toolkit**

- Tool: the repository's pytest and pre-commit commands plus a real cached crawl.
- What it produced: 508 exported features, passing checks and source-by-source ID comparison.
- Difficulties: a source schedule can be an empty list, and one latitude is invalid.
- What changed: handled the observed empty schedule and omitted the invalid coordinate pair.

## Implementation Notes

**2026-11-18**

Ran the scan, used the curated tracker, and checked current code to avoid completed or actively claimed work. Chose Pingo Doce #7950 and posted a bounded claim with a two-day report window.

**2026-11-18**

Verified the brand identity through Wikidata and NSI. Built the spider, discovered empty schedules in the first crawl, and handled the real source representation. A coordinate assertion then identified latitude zero for store 794, which I retained with null geometry.

**2026-11-18**

Committed the 25-line spider as 64d75bc. Ship-gate passed the manifest checks and PR precheck. Opened PR #20681 against upstream master and ran the PR monitor.

**Toolkit**

- Tool: local memory file, plan-check and ship-gate.
- What it produced: a one-file implementation traced to its plan and recorded deviations.
- Difficulties: Python entry-point scripts on the mounted filesystem were not executable.
- What changed: used uv run python -m invocations for the same installed tools.

## Pull Request

**PR Link:** https://github.com/alltheplaces/alltheplaces/pull/20681

**PR Description:** The description identifies the public source, explains missing coverage, records the 508-feature crawl and shared check outcomes, and discloses the empty schedules, invalid coordinate pair, and omitted holiday exceptions. It links issue #7950 and contains the required AI-use disclosure.

**Maintainer Feedback:**

- 2026-11-18: none yet from a human maintainer. An automated Copilot review reported an exhausted review quota. The monitor drafted a wait note, which I kept local.
- 2026-11-18: scraper-bot independently ran the spider and returned 508 items with zero errors and one warning. No response was needed to this automated result.

**Status:** Awaiting review.

**Toolkit**

- Tool: ship-gate followed by the voice guard on the Claude Code PR command.
- What it produced: PASS, then the upstream PR with exactly one spider file.
- Difficulties: the first direct grading attempts needed explicit access to installed skill evidence.
- What changed: supplied the installed directories and saved evidence before rerunning grading.

## Learnings and Reflections

**Technical Skills Gained**

I learned how JSONBlobSpider delegates common field parsing to DictParser and how the item pipelines apply country, brand and category data. I also learned to compare all source IDs and inspect individual output fields instead of relying on the item count.

**Challenges Overcome**

The first crawl exposed empty schedules. The coordinate check caught a zero latitude that a successful crawl alone would have missed. Checking the current source code also prevented me from claiming an open issue whose requested work already existed.

**What I'd Do Differently Next Time**

I would inspect the full source shape before writing the schedule loop and make default-branch coverage part of the scout's required checks. I would record the baseline in a saved script earlier so the complete verification command is easier to reuse.

## Resources Used

- AI301 Unit 5 through Unit 8 starter instructions for the scout, voice guard, ship-gate and monitor.
- https://github.com/alltheplaces/alltheplaces/issues/7950 for the public source and maintainer context.
- https://github.com/alltheplaces/alltheplaces/blob/master/AGENTS.md for spider design and validation rules.
- https://github.com/alltheplaces/alltheplaces/blob/master/docs/PULL_REQUEST.md for contribution guidance.
- https://www.wikidata.org/wiki/Q1575057 and the project's NSI command for brand verification.
- https://www.pingodoce.pt/wp-content/themes/pingodoce/ajax/pd-ajax.php?action=pd_stores_get_stores for source records.

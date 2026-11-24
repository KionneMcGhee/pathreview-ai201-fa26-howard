# PathReview Contribution Journal

## Unit 7 — Issue selection

**Issue link:** https://github.com/codepath/pathreview-ai201-fa26-howard/issues/63

**Issue title:** README scorer test fixture is too short for its own word-count assertion

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The README scorer's test for a high-quality README uses a short sample document but expects that sample to qualify as comprehensive. At present, the sample contains only about 51 words, although the scorer classifies a README as comprehensive only when it has at least 500 words. Because the fixture does not meet the test's stated conditions, the test fails against the intended scorer behavior instead of detecting an application bug. A successful fix would extend the representative sample with meaningful documentation, preserve its installation, usage, badge, demo, and tech-stack signals, and remove the expected-failure marker once the assertions pass.

**Issue selection notes — "Is this right for me?" scope check:**
- **Difficulty and experience:** Tier 1 is appropriate for my familiarity with Python and software testing and for an initial contribution to this repository.
- **Problem clarity:** The issue identifies the exact failed assertion and provides a concrete reproduction using `tests/unit/test_readme_scorer.py`.
- **Scope:** One focused test file; the task does not require changing scoring thresholds, production algorithms, database migrations, or frontend behavior.
- **Approach:** Expand the sample README to at least 500 words while retaining realistic installation, usage, features, stack, badges, and demo sections. Remove the issue #63 `pytest.mark.xfail(strict=True)` marker after the fixture passes.
- **Verification:** Run `pytest tests/unit/test_readme_scorer.py -q`, then the repository's recommended checks. These checks have not yet been completed.
- **Risk:** Making the fixture longer without satisfying the actual comprehensive threshold or forgetting to remove the xfail marker would leave this task incomplete.
- **Contribution fit:** This issue is small enough to understand and test independently, and it gives practice making focused, reviewable changes in a shared codebase.

**Branch name:** `test/63-readme-scorer-fixture`

**Setup confirmation:** [ ] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger (Howard tab, row 9)

**Issue claim comment:** [ ] Comment posted on the upstream GitHub issue (requires manual action; connected app does not have comment permission on the upstream repository)

**Local environment note:** The repository has not been cloned and launched on my Mac during this task; the localhost confirmation is deliberately left unchecked.

## Unit 8 — Reproduction & solution planning

**Reproduction commit** [`3d7a91a`](https://github.com/KionneMcGhee/pathreview-ai201-fa26-howard/commit/3d7a91a1494b5931e308d5d70302c5afa8d56ed6)

**Solution plan** [`PLAN.md`](https://github.com/KionneMcGhee/pathreview-ai201-fa26-howard/blob/test/63-readme-scorer-fixture/PLAN.md)

**Reproduction summary**

I ran the README scorer unit tests locally against the files from branch commit `37fe8a7` using Python 3.14.7, pytest 9.2.0, and structlog 25.5.0. The test module reported 22 passed and 1 expected failure. Running the targeted test with `--runxfail` surfaced the assertion failure `assert 51 > 100`. The scorer reported `word_count=51` and `category=minimal`, while the test is intended to exercise a comprehensive README.

**Reproduction commands**

From the repository root with the test dependencies available:

```bash
python -m pytest -q -rx tests/unit/test_readme_scorer.py
python -m pytest --runxfail -q tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals
```

The scorer counts whitespace-separated tokens with `len(content.split())` and uses 500 as the inclusive threshold for `comprehensive`. The failing fixture lives in `tests/unit/test_readme_scorer.py`. The count and category come from `ReadmeScorer._score_readme` in `agent/tools/readme_scorer.py`.

**Blockers or open questions**

The fixture should clear the 500-token cutoff with room for later edits. In Unit 9, I’ll confirm that the rewritten fixture still triggers every quality signal before removing the expected-failure marker.

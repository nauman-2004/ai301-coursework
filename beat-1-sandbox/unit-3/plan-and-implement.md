# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

nauman-2004

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/59#issuecomment-5984246709

I reproduced issue #59 with the `test_multiple_context_chunks` case: the current checker returns a faithfulness score of `0.0` where the test expects a score above `0.5`.

While planning the fix, I also checked the other issue #59 regression tests and found that claim extraction contributes to the problem. The partial-support input is currently extracted as one claim, which means the boolean support check can only give it a score of `0.0` or `1.0`, and the short `"Knows Rust"` claim is dropped by the current length filter.

My plan is therefore to keep the change inside `FaithfulnessChecker` but address both claim extraction and support matching. I will use the three existing issue #59 `xfail` tests as the main regression cases and preserve the existing negative tests so unrelated claims do not start counting as supported.

For verification, I will rerun my Unit 2 reproduction and expect its score to change from `0.0` to greater than `0.5`, then run the full `tests/unit/test_faithfulness_checker.py` test file with the issue #59 `xfail` markers removed once the behavior passes normally.

The exact extraction and local matching rules are still implementation choices I need to validate against both the supported and unsupported cases.

---

## Your branch

**Branch**

`fix/59-faithfulness-support`

**Evidence**

### Before

I reran the issue #59 regression tests before making the change:

```bash
python -m pytest \
  tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score \
  tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks \
  tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_claims_varying_support \
  --runxfail -v
```

Output:

```text
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score FAILED
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks FAILED
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_claims_varying_support FAILED

test_partial_support_returns_middle_score:
faithfulness_checked claims_count=1 score=0.0 supported_count=0
E       assert 0.2 < 0.0

test_multiple_context_chunks:
faithfulness_checked claims_count=1 score=0.0 supported_count=0
E       assert 0.0 > 0.5

test_multiple_claims_varying_support:
faithfulness_checked claims_count=2 score=0.0 supported_count=0
E       assert 0.2 < 0.0

3 failed
```

The direct Unit 2 reproduction was:

```bash
python - <<'PY'
from rag.evaluator.faithfulness_checker import FaithfulnessChecker

checker = FaithfulnessChecker()

feedback = "The developer has Python, JavaScript, and Docker experience."
context_chunks = [
    {"text": "Python expertise shown in backend projects."},
    {"text": "JavaScript skills demonstrated in frontend development."},
    {"text": "Docker and containerization knowledge evident in CI/CD pipelines."},
]

score = checker.check(feedback, context_chunks)
print("Faithfulness score:", score)
print("Expected: > 0.5")
PY
```

Before the change, its output was:

```text
faithfulness_checked claims_count=1 score=0.0 supported_count=0
Faithfulness score: 0.0
Expected: > 0.5
```

### After

I reran the same direct Unit 2 reproduction after the implementation:

```bash
python - <<'PY'
from rag.evaluator.faithfulness_checker import FaithfulnessChecker

checker = FaithfulnessChecker()

feedback = "The developer has Python, JavaScript, and Docker experience."
context_chunks = [
    {"text": "Python expertise shown in backend projects."},
    {"text": "JavaScript skills demonstrated in frontend development."},
    {"text": "Docker and containerization knowledge evident in CI/CD pipelines."},
]

score = checker.check(feedback, context_chunks)
print("Faithfulness score:", score)
print("Expected: > 0.5")
PY
```

Output:

```text
faithfulness_checked claims_count=3 score=1.0 supported_count=3
Faithfulness score: 1.0
Expected: > 0.5
```

I also reran the focused faithfulness-checker tests:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py -q
```

Output:

```text
....................x... [100%]
23 passed, 1 xfailed in 0.11s
```

The full unit suite finished with:

```text
380 passed, 50 xfailed, 2 warnings in 8.41s
```

The repository quality checks also passed:

```text
.venv/bin/ruff check .
All checks passed!

.venv/bin/black .
All done!

.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

My eval runs in order were: 1/1 on the initial `pkg-01` smoke test, 19/20 on the first full run, and 18/20 on the final full run saved to `eval-run.txt`.

**Package analysis**

I analyzed `pkg-14`. My rubric decided `reject`, while the gold label was `accept`. The package's diagnosis says that on reattach, Zellij sends OSC color queries and wires the client's input to the session before those responses are consumed. It grounds that diagnosis in the reproduced differences: fresh attach is clean, the leak begins on reattach, version 0.44.1 is clean with the same setup, and clearing the cache gives one clean attach before the leak returns.

My rubric was stricter about how much of that cause was directly established by the reproduction. It failed `Diagnosis grounded`, `Change targets cause`, `Approach executable`, and `Unknowns honest`, treating the proposed handshake explanation and exact implementation location as insufficiently verified. The gold label shows that this was too strict for a planning task: the package had enough evidence to identify a plausible implementation target, bounded the work to the reattach handshake, stated a concrete test plan, and explicitly identified the risk of consuming legitimate input. This package showed me the trade-off between requiring evidence-backed plans and demanding implementation-level certainty before the build has happened.

**Check rationale**

One of my current required checks is:

> | Unknowns honest | The plan's diagnosis, risks, unknowns, and implementation claims read against the reproduction evidence and available code facts, using the Risks and unknowns section of `references/evidence-guide.md` | Pass if material uncertainties or risks are stated honestly and the plan distinguishes verified facts from hypotheses that still need validation during implementation. Pass when no material unknown remains and the evidence supports that certainty. Fail if an unresolved assumption could change the implementation or outcome but is presented as established fact or omitted. | required |

I made this required because a plan can look executable while still depending on an assumption that has not been established by the reproduction or available code evidence. I wanted the check to distinguish verified observations from hypotheses instead of requiring every diagnosis to be proven before implementation. The explicit sentence that a plan can pass when it honestly states material uncertainty is important because planning happens before every implementation detail is known. `pkg-14` also showed the danger of applying this check too aggressively: a plausible diagnosis supported by several controls should not automatically fail just because implementation still needs to confirm the exact mechanism.

**Trade-offs**

The trade-off in `Unknowns honest` is that it can reject a plan that is actually ready to build when the diagnosis is necessarily provisional. `pkg-14` is the clearest example: the gold label was `accept`, but my rubric rejected it partly because it treated the proposed reattach-handshake cause as too certain. I accept some strictness because hiding an outcome-changing assumption can send an implementation in the wrong direction, but the check should not require proof that can only come from doing the implementation itself.

My final full run still matched 18/20 packages and every evaluation category had at least one match, so the rubric passed the evaluation bar even with this trade-off.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

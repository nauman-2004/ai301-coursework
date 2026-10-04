# Plan for issue #59

## Diagnosis

My reproduction showed that `test_multiple_context_chunks` is still an expected failure and that running the same input directly through `FaithfulnessChecker.check()` produces:

```text
faithfulness_checked claims_count=1 score=0.0 supported_count=0
Faithfulness score: 0.0
Expected: > 0.5
```

The current `_is_supported()` implementation tokenizes the claim and context using `lower().split()`, removes stop words from their literal token overlap, and requires at least two remaining shared tokens. This makes support depend on shared wording rather than whether the context contains evidence for the claim. It also makes short claims especially difficult to mark as supported.

I also verified that claim extraction affects two of the issue #59 regression tests. `_extract_claims()` splits only on sentence punctuation and drops extracted strings whose stripped length is not greater than 10 characters. For the partial-support test, the Python and Kubernetes statements remain one extracted claim, so the current boolean support decision can only produce a score of `0.0` or `1.0`, not the expected middle score. In the varying-support test, `"Knows Rust"` is dropped by the length filter, leaving only two extracted claims. This means an `_is_supported()`-only change cannot satisfy all three issue #59 tests.

The issue #59 tests show the behavior that the change needs to support: partial support should produce a middle score, evidence spread across multiple context chunks should count as support, and short Python/Docker claims should be recognized while an unsupported Rust claim remains unsupported.

The exact replacement matching rule still needs to be validated during implementation. I do not want to assume that simply lowering the overlap threshold or adding a heavier semantic dependency is correct before checking it against the existing supported and unsupported cases.

## Scope

In scope:

- Update the claim extraction and support-matching behavior used by `FaithfulnessChecker` so short or differently worded supported claims can contribute correctly to the faithfulness score.
- Preserve the existing behavior that unrelated claims are not marked supported.
- Update the issue #59 unit tests so they become normal passing regression tests once the behavior is fixed.
- Add or adjust focused unit coverage if needed to distinguish differently worded support from unrelated context.

Out of scope:

- Redesigning the overall faithfulness scoring API.
- Changing the RAG retrieval or vector-store system.
- Adding an LLM call or external service to the faithfulness checker.
- Fixing unrelated faithfulness-checker issues such as issue #60.
- Changing the scoring ranges expected by tests except where necessary to make issue #59's intended behavior testable.

## Files to touch
- `rag/evaluator/faithfulness_checker.py` — adjust `_extract_claims()` and `_is_supported()` as needed so claim granularity and support matching satisfy the issue #59 behavior while preserving unsupported-claim detection.
- `tests/unit/test_faithfulness_checker.py` — remove the issue #59 `xfail` markers after the fix and keep/add regression coverage for supported and unsupported wording.

I do not currently expect `rag/evaluator/eval_suite.py` to require a change because it only constructs and calls `FaithfulnessChecker`; it does not implement the support decision.

## Approach

1. Use the three existing issue #59 `xfail` tests as the primary behavioral constraints and inspect both stages that affect their scores: claim extraction and support matching.
2. Adjust `_extract_claims()` so the issue #59 inputs preserve the meaningful claims needed for partial-support scoring, including short claims such as `"Knows Rust"`, without turning empty or meaningless fragments into claims.
3. Adjust `_is_supported()` so support does not depend solely on two exact shared non-stop-word tokens. Keep the implementation local and deterministic rather than introducing the retrieval system, an external model, or another service.
4. Validate the extraction and matching changes together against the existing positive and negative tests, especially unrelated Rust/Python context, case-insensitive matching, specialized technical terms, empty inputs, and the three issue #59 regression cases.
5. Once all three issue #59 cases pass with the intended behavior and existing negative cases remain correct, remove the three strict `xfail` markers associated with issue #59.
6. Run the focused faithfulness-checker test file to check for regressions.

The exact extraction and local matching rules are implementation details I still need to validate. I will choose them based on the issue #59 regression cases and existing positive and negative tests rather than treating one proposed algorithm as proven in advance.

## Test plan

Before the fix, my Unit 2 reproduction produced:

```text
faithfulness_checked claims_count=1 score=0.0 supported_count=0
Faithfulness score: 0.0
Expected: > 0.5
```

After the fix I will rerun the same direct `FaithfulnessChecker.check()` input. The observable result should change from `0.0` to a score greater than `0.5`.

I will also rerun:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py -q
```

The three tests currently marked `xfail` for issue #59 should pass normally after their `xfail` markers are removed, while the existing unsupported-context tests should continue to pass.

I will also run the specific reproduction target:

```bash
python -m pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks -v
```

Expected after the fix: the test passes and its `score > 0.5` assertion succeeds rather than reproducing the previous `0.0` result.

## Risks and unknowns

- A matching rule that is too permissive could incorrectly mark unrelated claims as supported.
- A rule based only on lowering the exact-word threshold may make short claims pass for the wrong reason, so I need to validate positive and negative cases together.
- Adding embeddings or an external model would increase complexity and dependencies for a small evaluator, so I will avoid that unless the existing local logic cannot satisfy the required behavior.
- The exact local matching strategy is still an implementation choice to validate against the tests rather than a verified part of the diagnosis.
- Changing claim extraction could alter the denominator used to calculate faithfulness scores, so existing scoring behavior must be checked for regressions.
- The right granularity for a sentence containing multiple skill assertions is still an implementation decision to validate; the current reproduction proves the existing granularity is insufficient for the partial-support test, but it does not by itself establish the best splitting rule.
- Short claims need to remain extractable without admitting empty or meaningless fragments.

## Deviations

The implementation stayed within the planned production file and test file, but the exact extraction and matching rules were refined during the build.

For claim extraction, I split sentence-level feedback further on comma-separated and `and`-joined assertions and removed the previous `len > 10` filter so short claims such as `Knows Rust` are preserved.

For support matching, I changed tokenization from whitespace splitting to a regex that preserves internal `.` or `-` characters while excluding sentence-ending punctuation. I also changed the support decision so a focused extracted claim can be supported by one meaningful shared token. The stop-word set was extended with `developer`, `has`, `shows`, `with`, and `skills` so generic shared wording does not establish support by itself.

A post-build review exposed two edge cases that were not covered by the original three issue #59 tests. Through the public `check()` path, `"Uses Kubernetes."` against `"Deployed services on Kubernetes."` initially scored `0.0` because sentence punctuation remained attached to the context token, while `"Strong communication skills."` against `"Python skills demonstrated."` incorrectly scored `1.0` because `skills` alone satisfied the one-token rule. I added regression tests for both cases, reproduced both failures, then corrected the tokenizer and stop-word handling. Both new tests now pass along with the original three issue #59 regression tests.

The post-build review also found a mypy error for the newly introduced `claims` list. I reproduced it and added the `list[str]` annotation required by the repository's type checker.

The three issue #59 tests now pass normally with their strict `xfail` markers removed. Issue #60 remains unchanged and expected to fail. The exact Unit 2 reproduction now reports `claims_count=3`, `supported_count=3`, and `score=1.0` instead of `score=0.0`.

Final validation produced `23 passed, 1 xfailed` for `tests/unit/test_faithfulness_checker.py` and `380 passed, 50 xfailed` for the full unit suite. `make check` also passes Ruff, Black, and mypy with no errors.

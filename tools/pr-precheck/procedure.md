# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the issue context first. Record the reported problem, reproduced trigger, expected behavior, and any maintainer guidance that constrains the submitted change.
2. Read the repository requirements next. In eval mode, record the repo-facts block's PR-template asks, contribution rules, testing requirements, branch or issue-linking requirements, and disclosure policy. In live mode, record the equivalent requirements from the PR template and `docs/CONTRIBUTING.md`.
3. Read `plan.md` before reading the diff. Record the planned scope, files or areas to change, implementation approach, test plan, non-goals, and every entry under `## Deviations`. This establishes the implementation boundary before seeing what the branch actually changed.
4. Read the committed branch diff next. In live mode use `git diff main...HEAD`; in eval mode use the bundle's diff. Record every changed file and the material purpose of each hunk. Compare these facts with the plan only after recording them.
5. Read the test evidence next. Record the before reproduction, after reproduction, repository-required check commands, their outputs, and any honestly disclosed failure or unavailable check.
6. Read the draft PR title and description last. Record each material claim about the issue, implementation, testing, limitations, and deviations. Reading the description after the diff prevents its wording from substituting for what the branch actually changes.
7. Do not grade any rubric check until all required package parts have been read and these facts have been recorded.

## Evidence gathering

1. For **Plan fidelity**, pair every material changed file and diff hunk with the plan's scope, approach, or `## Deviations`. Record any diff behavior with no plan/deviation support and any planned outcome the PR claims complete but the diff does not implement.
2. For **Description matches change**, pair each material title or description claim with the diff and plan. Record whether the claimed behavior, files, limitations, and deviations are actually present in the committed change.
3. For **Evidence proves behavior**, pair the plan's test plan and Unit 2 reproduction trigger with `test_evidence.md` or the eval bundle's evidence. Record the before result, after result, every repository-required check, and its reported outcome. When evidence quotes a command result, use the supplied command/output itself as the evidence rather than trusting a summary sentence.
4. For **Diff reviewable**, inspect every changed file and material hunk. Record debug statements, commented-out experiments, generated or accidental files, unrelated cleanup, or other changes that do not contribute to the issue or a documented deviation.
5. For **Standards satisfied**, collect the applicable PR-template fields, contribution rules, branch naming rules, issue-linking requirements, required test reporting, and disclosure requirements. Compare them with the branch name, draft title and description, and test evidence. Record every requirement as satisfied, honestly marked inapplicable, or missing.
6. In live mode, gather issue-side and repository-policy evidence only from the sources named by `SKILL.md` and `references/evidence-guide.md`. In eval mode, use only the bundle and never fetch or infer missing facts from outside it.

## Check execution

1. Grade checks in rubric order: **Plan fidelity**, **Description matches change**, **Evidence proves behavior**, **Diff reviewable**, then **Standards satisfied**.
2. For each check, use only the evidence gathered for that check and apply the exact pass and fail conditions in `rubric.md`. Do not replace them with a general impression of whether the PR seems good.
3. Grade `pass` when the evidence satisfies the pass condition and does not demonstrate a listed fail condition.
4. Grade `fail` when the gathered evidence demonstrates a condition that the rubric explicitly says should fail.
5. Grade `unclear` when evidence required to decide the check is genuinely absent or insufficient. Do not use `unclear` merely because a disclosed shortfall exists; evaluate an honestly disclosed shortfall under the check's stated pass condition.
6. Reuse recorded facts across checks when appropriate, but apply each check independently. For example, an undocumented extra file may fail both Plan fidelity and Diff reviewable for different reasons.
7. When the package states that a test, command, or validation produced a result, compare that claim with the supplied command/output evidence when available. If the claim and evidence conflict, grade from the observable evidence and record the contradiction.
8. Record one concise fact or quote that directly explains every grade. For `fail` or `unclear`, record the missing, contradictory, undisclosed, or unrelated evidence that caused the result.

## Verdict assembly

1. After every rubric check has a grade, apply the verdict rule from `rubric.md` exactly as written.
2. Return `accept` only when every required check is `pass`.
3. Return `reject` when any required check is `fail` or `unclear`. Preferred checks, if later added, never change the verdict.
4. An honestly disclosed shortfall does not change the verdict by itself; its check grade depends on whether that check's pass condition allows the disclosed state.
5. For a rejected package, identify the first failed or unclear required check in rubric order as the primary deciding check in the readable summary, then report any additional failed or unclear checks as well.
6. Report every check's grade with one concise evidence fact or quote.
7. End with the fixed JSON schema required by `SKILL.md`, listing every check in rubric order and setting `"verdict"` to only `"accept"` or `"reject"`. The JSON block must be valid and must be the final content in the output.

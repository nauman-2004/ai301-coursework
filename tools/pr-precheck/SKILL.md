---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

Grade exactly one PR package to answer one question: **is this ready to submit?**

A PR package consists of the candidate PR title and description, the committed branch diff, test evidence, and the plan the change claims to implement, including any recorded deviations. Read those artifacts against the issue the plan belongs to and the repository requirements that apply to the submission.

Do not grade unrelated code quality, redesign the implementation, or answer a different question. The rubric and evidence determine whether the package is ready.

## Inputs and modes

Operate in exactly one of two modes.

**Live mode:** Grade the student's own pending PR before it is opened. Read:

- `plan.md`, including its `## Deviations` section;
- the committed branch diff relative to `main`, obtained from `git diff main...HEAD`;
- the draft PR title and description from `pr_draft.md`;
- the test commands, outputs, and before/after evidence from `test_evidence.md`;
- the issue URL supplied by the student.

Gather issue-side evidence from the live Path Review repository: the issue and its thread, the repository's PR template, and `docs/CONTRIBUTING.md`. Compare the draft description and test evidence with the actual committed diff rather than with uncommitted working-tree changes.

For a house-chain student, use the supplied house plan and house reproduction pack in place of the student's own plan and reproduction evidence. Grade the resulting package with the same rubric and procedure.

**Eval mode:** Grade exactly one provided package bundle. Treat the bundle as the whole world: use only facts contained in it, fetch nothing from GitHub or the local repository, and do not supplement missing evidence from outside sources. Run every rubric check and apply the complete verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before gathering or grading any other evidence. Confirm that the issue and pending PR belong to the repository named by its `Repo:` line, and apply the house rules stated there when interpreting the package.

If the issue or PR is outside the scoped repository, refuse to grade it. If the `Repo:` line still contains an unfilled placeholder, stop without grading and tell the student to fill that line with their section's Path Review repository. Never guess or infer the missing scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` before judging outgoing PR text. Check both the draft PR title and description against the student's rules there.

If the title or description breaks a voice-guide rule, report the broken rule in the readable summary and identify the draft text that conflicts with it. Do not let a voice-guide violation change the rubric verdict by itself unless a rubric check explicitly uses that evidence.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` to determine the checks, their evidence requirements, pass conditions, weights, and the final verdict rule.

Use `references/evidence-guide.md` as the map for locating each evidence family in either a live PR package or an eval bundle.

Execute `procedure.md` exactly as written. It determines the read order, evidence-gathering steps, check execution, and verdict assembly. Do not silently invent a missing procedural step. If the procedure is silent about something required to execute a check, report that procedure gap in the readable summary.

Before grading, confirm that `rubric.md` contains student-authored checks and that `procedure.md` contains student-authored operating steps. If either has no substantive content beyond its template comments, refuse to grade and explain which component is incomplete.

## Verdict and output

The verdict space is binary. Return `accept` only when the package is ready to submit under the rubric's verdict rule. Return `reject` when the rubric says the submission must be held. Do not invent a third verdict or attach conditions to `accept`.

A short readable summary may appear before the machine-readable result. End every completed grading run with the fenced JSON block below. It must be valid JSON, contain every rubric check in rubric order, and be the final content in the response. Nothing may appear after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Grade evidence first. Every check must name the concrete fact, artifact, command result, diff fact, or quote that decided its grade. Never use a general impression such as "looks good" as evidence.

Grade the submission itself, not its polish. A concise PR may be complete, while a detailed PR may still contain silent drift, missing tests, unreviewable changes, or repository-policy violations.

Let `rubric.md` decide each check. If the evidence satisfies the written pass condition, grade it accordingly even when intuition disagrees. Fix disagreements by revising the rubric between runs, not by improvising a different rule during grading.

Let `procedure.md` decide how evidence is gathered and compared. Follow it exactly. When it leaves a necessary step undefined, report the gap rather than silently inventing a workflow.

Use `unclear` only when the evidence needed to decide a check is genuinely absent or insufficient. Apply the treatment of `unclear` written in the rubric's verdict rule. If the rubric is silent, treat `unclear` as failing for verdict purposes.

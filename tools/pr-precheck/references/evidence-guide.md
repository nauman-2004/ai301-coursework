# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives:** In eval mode, read the plan-context block's scope, implementation approach, non-goals, and deviation notes against the Candidate PR's unified diff, changed files, commits, title, and description. In live mode, read `plan.md`, especially its scope and `## Deviations`, against `git diff main...HEAD`, then compare the material claims in `pr_draft.md` with that same committed diff.

**What good looks like:** Every material changed file and behavior is covered by the original plan or an explicit deviation that says what changed and why, and the PR does not silently leave out work it claims to have completed. The title and description describe the implementation that is actually in the diff; an honestly recorded change of approach or shortfall is not silent drift.

## Test evidence (harness category: not-tested)

**Where it lives:** In eval mode, read the plan-context block's test plan and reproduction evidence against the Candidate PR's test-evidence section, including commands, before/after output, and repository-check results. In live mode, read the test plan in `plan.md` and the Unit 2 reproduction against `test_evidence.md`, including the re-run reproduction and the outputs of `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`.

**What good looks like:** The evidence exercises the reproduced bug through an observable before/after result that would differ when the fix works, and it reports every testing or validation check explicitly required by the repository facts. In live Path Review mode, this includes the four checks named by its PR template. A failing or unavailable check can still be useful evidence when its command, output, and reason are shown honestly; a summary such as "tests pass" without observable supporting output is not enough.

## Diff quality (harness category: unreviewable)

**Where it lives:** In eval mode, inspect the Candidate PR's unified diff, changed-file list, and commit list, read against the issue and plan context. In live mode, inspect `git diff main...HEAD` and the branch's commits, using `plan.md` and its deviations to distinguish intentional implementation work from unrelated changes.

**What good looks like:** The intended fix is easy to isolate and the changed files and hunks contribute to the issue or a documented deviation. Debug prints, temporary instrumentation, commented-out experiments, dead code, accidental generated files, broad formatting churn, and unrelated cleanup are absent; if unrelated work rides along, the diff is not reviewable merely because the description mentions it.

## Standards and comms (harness category: standards-wall)

**Where it lives:** In eval mode, read the repo-facts block's PR-template requirements, contribution instructions, branch or issue-linking rules, testing asks, maintainer guidance, and stated disclosure policy against the Candidate PR's title, description, branch facts, and test evidence. In live mode, read the repository's `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, relevant issue-thread guidance, branch name, `pr_draft.md`, and `test_evidence.md`.

**What good looks like:** Every applicable repository requirement is satisfied with truthful content or explicitly marked inapplicable when the repository allows that. The PR identifies the issue in the required form, reports the requested testing state, follows branch and contribution rules, engages material maintainer direction, and includes any required disclosure such as AI use. Boilerplate, falsely checked boxes, or silently omitted required sections do not satisfy the standard.

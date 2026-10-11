# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | The committed branch diff read against the plan's scope, implementation approach, and `## Deviations`, using the Plan fidelity section of `references/evidence-guide.md` | Pass if every material change in the diff is covered by the posted plan or an honestly recorded deviation, and the diff does not silently omit work that the PR claims to have completed. A disclosed shortfall or changed approach may pass when the plan records what changed and why. Fail if the diff silently adds unrelated or unplanned behavior, materially departs from the plan without a deviation, or claims completion while leaving planned work out without disclosure. | required |
| Description matches change | The draft PR title and description read against the committed diff, the plan, and its deviations, using the Plan fidelity and Standards and comms sections of `references/evidence-guide.md` | Pass if the title and description accurately state what the committed diff changes, what issue it addresses, and any material limitation or deviation relevant to review. Fail if the description claims behavior the diff does not implement, hides material changes or omissions, or describes a materially different change from the one actually submitted. | required |
| Evidence proves behavior | The test evidence read against the plan's test plan, the issue's reproduced behavior, and the committed diff, using the Test evidence section of `references/evidence-guide.md` | Pass if the evidence includes observable before/after results for the reproduced issue and every check explicitly required by the repository facts was run with its outcome reported. A failing or unavailable check may still pass this check when its result and reason are disclosed honestly and the evidence is sufficient for a reviewer to understand the submission's tested state. Fail if the claimed fix is not exercised observably, checks explicitly required by the repository facts are silently omitted, outputs do not support the claimed result, or the evidence could look the same while the reported bug remains. | required |
| Diff reviewable | The committed diff's changed files and hunks read against the issue, plan, and deviations, using the Diff quality section of `references/evidence-guide.md` | Pass if the diff is limited to changes relevant to the issue and its documented implementation, with no debug output, commented-out experiments, generated debris, accidental files, or unrelated cleanup that materially distracts from review. Fail if unrelated or leftover changes make the intended fix harder to isolate or introduce work that does not belong to the issue. | required |
| Standards satisfied | The draft PR title and description, test evidence, branch name, and package facts read against the repository's PR template, contribution policy, and applicable disclosure requirements, using the Standards and comms section of `references/evidence-guide.md` | Pass if the submission satisfies the repository's stated PR requirements or honestly marks an inapplicable item as such, names the issue as required, and includes every required disclosure such as AI use. Fail if a material template, contribution-policy, branch, issue-linking, testing-report, or disclosure requirement is ignored, falsely claimed, or silently omitted. | required |

## Verdict rule

Accept only if every required check passes. Reject if any required check fails or is unclear. Preferred checks, if added later, never change the verdict. An honestly disclosed shortfall is not automatically a failure; grade it by the individual check's pass condition and whether the package gives a reviewer enough truthful evidence to understand the submitted state.

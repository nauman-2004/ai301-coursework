# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Issue unclaimed | "this issue: assignees" and "linked PRs" under Repo facts, plus the Comments section for claim comments and later claim-status updates | Pass if there is no current assignee, no open linked PR, and no claim in the Comments section that remains active. A claim does not count as active if later evidence explicitly shows it was abandoned, unassigned, released, or otherwise no longer valid. | required |
| Repo in use | The repo's "archived" status and "last 5 default-branch commits" under Repo facts; measure commit recency against the bundle capture date in eval mode and today in live mode | Pass if the repo is not archived and at least one of the last 5 default-branch commits is within 90 days of the reference date. | required |
| Maintainer alive | The "last 5 default-branch commits" and "maintainer first-response sample" under Repo facts; inspect commit authors and owner/member/collaborator responses | Pass if at least 1 of the last 5 default-branch commits within 90 days of the reference date provides evidence of human maintainer activity, OR the maintainer first-response sample contains at least 1 owner/member/collaborator response. Bot-authored commits alone do not count as human maintainer activity. | required |
| Scope fits newcomer | The Issue body and Comments section, plus "linked PRs" under Repo facts; look for the requested deliverable, umbrella/tracking language, whether listed ideas are required work or only suggestions, unresolved design/product decisions, maintainer statements about core internals, and repeated abandoned attempts. | Pass if the issue defines a bounded deliverable or coordinated changes serving one defined outcome. Suggestions or optional approaches do not count as required work unless the issue or a maintainer says they are required. Fail if the issue is an umbrella/tracking issue containing independent work items, unresolved design or product decisions prevent knowing what to implement, a maintainer says the work requires changes to core internals, or the history shows repeated abandoned attempts such as multiple closed unmerged PRs or multiple contributors claiming the issue and failing to complete it. | required |

## Verdict rule

Accept only if every required check passes. Reject if any required check fails or is unclear. Preferred checks do not affect the accept/reject verdict and are used only to rank accepted issues.
<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

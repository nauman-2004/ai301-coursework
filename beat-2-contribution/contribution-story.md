# Contribution: Add Loading States and Skeleton Components

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Nauman Sayed
**GitHub Username:** nauman-2004
**Issue:** https://github.com/NITRR-Official/CampusOS/issues/12
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built an issue scout to search, rank, and grade open-source issues**

I built my issue scout around my Python, TypeScript, and full-stack experience, using good first issue and help wanted searches and ranking unassigned issues with fewer comments higher. The first scan graded all five candidates consistently, so I did not change the rubric, but four of the five results came from the same repository, which showed that my ranking strategy can overrepresent one active project. I then used the scout to grade issues from the course Issue Selection site and chose NITRR-Official/CampusOS #12 because the scout found it unclaimed, active, maintained, and suitable for a newcomer.

**Unit 6: Added a voice guard for upstream posts and a local CampusOS repo memory**

I extended my voice guide with rules for claims, maintainer questions, and follow-up pings, then installed the voice guard as a Claude Code hook. The wiring test correctly blocked a body-less gh issue comment command, and my real CampusOS claim passed both repro-check and the guard before it was posted. I also created a local CLAUDE.local.md after a scoped frontend orientation; it records the CampusOS contribution rules and loading-state architecture, including the unresolved question about whether issue #12 should use the design-system package or the duplicate frontend components.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue through the AI301 Issue Selection site after searching for issues that matched my experience with React, TypeScript, and full-stack development. I initially considered CampusOS #12 and Apache DataFusion Ballista #2604. After patching my scout's scope check for real-world issues, I graded CampusOS #12 again. The scout accepted it because it was unclaimed, the repository was still active, a maintainer had responded recently elsewhere in the repository, and the issue described one identifiable loading-state improvement.

The scout rated the issue as **ambitious**, mainly because the exact scope is not completely clear. The repository has UI primitives in both the frontend and design-system areas, and the phrase "across the application" could make the task much larger than the specific button, card, and list loading states named in the issue. I still chose it because the React/UI work fits my existing full-stack experience, while giving me experience working in an unfamiliar open-source codebase. I raised those scope questions in my claim instead of assuming the intended design.

One limitation I found in my scout was that my initial search ranking heavily favored one active repository, with four of five scan results coming from the same project. I also found that my original `Scope fits newcomer` check was too strict for real open-source issues, where some implementation details are often intentionally left open. In Unit 6 I replaced it with `scope-feasible`, which rejects true umbrella issues but moves open design edges into a difficulty note instead of automatically rejecting the issue.

**Claim comment:** https://github.com/NITRR-Official/CampusOS/issues/12#issuecomment-6588818845

**Toolkit**

- Tool: issue-scout; I used my profile-based search strategy and rubric to scan open-source issues and then grade CampusOS #12 directly.
- What it produced: The scout accepted CampusOS #12 on all four required checks and rated it ambitious because the extent of the loading-state work and canonical component location need clarification.
- Difficulties: My initial scan overrepresented one active repository, and my original Scope fits newcomer check was stricter than real-world issues required.
- What changed: I replaced Scope fits newcomer with the Unit 6 scope-feasible check and moved open design questions into a separate difficulty note instead of using them as automatic rejection reasons.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I reproduced the issue on macOS using my fork of NITRR-Official/CampusOS, based on the repository's dev branch at `42634891f02a6538043d15cfbf97845a25a94aa3` and working on feature/12-loading-states. The project uses pnpm, and pnpm install completed successfully. Since issue #12 describes a missing/inconsistent UX feature rather than a crash, I reproduced it by inspecting the current loading components and their usage in the codebase.

**Steps to Reproduce**

1. Inspect packages/design-system/src/components/ui/button.tsx. The shared Button supports its normal variants and sizes but has no standard loading prop or built-in spinner behavior.
2. Inspect packages/design-system/src/components/ui/skeleton.tsx and search for <Skeleton> usages. A low-level Skeleton primitive already exists, but pages such as the admin clubs, plugins, users, and events pages manually compose multiple Skeleton elements with page-specific dimensions.
3. Run find frontend/app -name 'loading.tsx' -print. The only result is frontend/app/(dashboard)/loading.tsx, showing that route-level loading coverage is currently limited to the dashboard route group.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** Code inspection showed the shared Button has no loading prop, multiple pages manually compose Skeleton elements, and only frontend/app/(dashboard)/loading.tsx exists as a route-level loading file.
- **My findings:** I reproduced the feature gap described by #12, but found an important detail: CampusOS already has a basic Skeleton primitive. The missing behavior is therefore consistent and reusable loading patterns rather than skeleton support being completely absent. I also found duplicate UI component locations, so I asked the maintainer which location should be canonical before implementing the change.

**Toolkit**

- Tool: repro-check; I used it first on my claim draft and then on the claim together with my reproduction report.
- What it produced: It verified that my environment and inspection commands were followable and that the observed Button, Skeleton, and route-loading behavior directly matched the issue's requested improvements.
- Difficulties: The issue's broad wording could have led me to say that CampusOS had no skeleton support, but inspection showed that a basic Skeleton primitive already exists.
- What changed: I described the problem more precisely as inconsistent and non-reusable loading behavior and kept the component-location question open for the maintainer instead of assuming a solution.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

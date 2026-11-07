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
**Pull Request:** https://github.com/NITRR-Official/CampusOS/pull/137

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

**Unit 7: Used ship-gate to validate my implementation before opening the PR**

I installed ship-gate and created a manifest using CampusOS's lint, type-check, and build checks, while recording the pre-existing pnpm test MongoMemoryServer failure separately. My first ship-gate run stopped because my test evidence only described the UI behavior instead of proving it, so I added rendered-DOM verification for the Button and CardSkeleton. Ship-gate then passed all repository checks and all five pr-precheck checks, and I opened the PR through Claude Code.

**Unit 8: Added a PR monitor and used it to handle maintainer feedback**

I created a playbook with rules for review responses, staleness, nudges, and when to walk away, then ran the PR monitor on PR #137. It detected the maintainer's request to resolve conflicts and drafted an acknowledgement that did not claim the work was already fixed. I chose not to post that draft immediately; I first rebased onto the latest dev, resolved the conflict, reran the required checks, pushed the updated branch, and then wrote my own reply through the voice guard. The monitor now reports the PR as mergeable with two passing checks and correctly recommends waiting rather than sending another message.

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

CampusOS has several different ways of showing loading states instead of one consistent pattern. A basic Skeleton component already exists, but pages manually combine Skeleton elements for cards and lists, and the shared Button does not have a standard loading state.

**Expected Behavior**

Loading feedback should use reusable components and feel consistent with the existing UI. Common content such as cards and lists should be able to use reusable skeleton layouts, buttons should have a standard loading state, and page-level loading UI should be used where appropriate.

**Current Behavior**

The shared Button has no loading prop or built-in spinner. Several pages manually build their own skeleton layouts with repeated Skeleton elements, and only the dashboard route group currently has a loading.tsx. This means different parts of the application use different loading patterns.

**Affected Components**

The main shared components are packages/design-system/src/components/ui/button.tsx, skeleton.tsx, and card.tsx, along with route-level loading UI such as frontend/app/(dashboard)/loading.tsx. There is also an older duplicate component tree under frontend/components/ui, so I asked the maintainer which location should be canonical before making that broader cleanup part of the change.

**Toolkit**

- Tool: repro-check; I used it to validate my claim and code-level reproduction against issue #12.
- What it produced: It confirmed that the missing Button loading state, repeated page-specific Skeleton layouts, and limited route-level loading coverage directly support the issue.
- Difficulties: The issue broadly says there is no clear loading feedback, but inspection showed that CampusOS already has a basic Skeleton primitive and several existing loading patterns.
- What changed: I narrowed my description from “missing loading support” to inconsistent and non-reusable loading behavior, and I kept the duplicate-component question separate instead of assuming it should be cleaned up in this PR.

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

The issue is caused by loading behavior being implemented independently at individual call sites instead of through shared UI patterns. The design-system Button renders its normal children and props but has no loading state, so forms handle pending states themselves. The existing Skeleton is only a low-level primitive, which causes pages to manually compose repeated skeleton layouts. Page-level loading is also inconsistent, with only the dashboard route group currently providing a Next.js loading.tsx.

Most of the current application uses @campusos/design-system, so that package appears to be the current shared-component location. However, the repository also contains older duplicate components under frontend/components/ui. I asked the maintainer which location should be canonical and have not received a response yet.

**Proposed Solution**

I will keep the first implementation bounded to reusable loading behavior in the existing design-system package. I plan to add a standard loading state to the shared Button and add a lightweight reusable skeleton composition for the common card loading layout. I will then use the shared loading components in a small representative part of the existing UI so the behavior is demonstrated without refactoring every loading state in the application.

I will not remove or migrate the duplicate frontend/components/ui tree, refactor every existing loading state, or introduce a new frontend testing framework as part of this issue. If the maintainer gives different direction about the canonical component location before the implementation is finished, I will update the plan and record the deviation.

I will not add new route-level loading.tsx files in this first change. The dashboard already has route-level loading, and deciding which additional route groups need it would expand the scope beyond the reusable component work. I will leave additional page-level loading coverage for follow-up or maintainer direction.

**Implementation Plan**

1. Modify `packages/design-system/src/components/ui/button.tsx` to add an optional loading state. When loading, the Button will be disabled, expose `aria-busy`, and show a spinner while preserving its existing behavior when loading is false.
2. Add a reusable card skeleton composition alongside the existing design-system Skeleton primitive and export it through the design-system package's existing public API.
3. Replace the hand-built card-shaped skeleton loading UI in `frontend/app/(dashboard)/admin/plugins/page.tsx` with the reusable skeleton composition as the representative skeleton integration. Keep the table-based admin loading states unchanged because they would require a separate table-row skeleton design.
4. Use the shared Button loading state on the existing Approve action in the admin clubs UI. The current implementation already disables the button and manually renders a `Loader2` spinner while `actionLoading` is true. After the change, the shared Button should provide that spinner/disabled behavior instead of the call site, and the rendered button should additionally expose `aria-busy` while loading.
5. Manually verify the admin plugins loading state before and after the change. Before the change, the page manually composes individual Skeleton elements; after the change, it should render the reusable card skeleton composition while preserving the same loading purpose.
6. Run `pnpm format`, `pnpm lint`, `pnpm type-check`, and `pnpm build`. Re-run `pnpm test` and report its result separately; the pre-implementation baseline already failed because multiple backend plugin suites could not start `MongoMemoryServer` within 10000ms.

**Non-goals:** I will not migrate the duplicate `frontend/components/ui` tree, refactor every existing loading state, change the table-based admin skeleton layouts, add new route-level `loading.tsx` files, or introduce a new frontend testing framework in this PR.

No implementation deviations yet. The maintainer clarification about the canonical component location and intended extent of "across the application" is still pending.

**Toolkit**

- Tool: plan-check; I used it to compare my proposed fix with the reproduced behavior, issue scope, repository code, and maintainer thread. The final plan passed all seven checks and bounded the work to a shared Button loading state, a reusable card skeleton, and representative integrations in the admin clubs and admin plugins pages.
- What it produced: The final plan passed all seven checks and bounded the work to a shared Button loading state, a reusable card skeleton, and representative integrations in the admin clubs and admin plugins pages.
- Difficulties: My first plan did not decide whether page-level loading was in scope, and a later version tried to use a card/list skeleton on a table-based loading state. Plan-check also found that the Approve button already had a hand-written spinner and disabled state.
- What changed: I explicitly deferred additional route-level loading, moved the skeleton integration to the card-shaped admin plugins page, narrowed the reusable skeleton to a card skeleton, and made aria-busy plus removal of the hand-written spinner part of the observable Button change.

---

## Testing Strategy

I verified the change against the same loading-state gaps I identified before implementation. For the shared Button, the admin clubs Approve action previously handled its pending state locally with disabled and a manually rendered Loader2 spinner. After the change, it uses the shared loading prop, and the Button provides the disabled state, spinner, and aria-busy behavior. For skeleton loading, the admin plugins page previously composed individual Skeleton elements for every loading card; after the change, it uses the reusable CardSkeleton exported by the design system.

I also ran the repository checks. pnpm format, pnpm lint, pnpm type-check, and pnpm build pass. During validation, the first production build exposed a regression in the new Button implementation because Button asChild received an extra child from the conditional spinner. I corrected that behavior and reran the build successfully, including all 21 generated static pages. pnpm test still fails because several backend plugin suites cannot start MongoMemoryServer within 10000ms; I reproduced the same failure before implementing issue #12. The frontend test script itself reports No tests yet, so I did not introduce a new frontend testing framework for this PR.

**Toolkit**

- Tool: ship-gate manifest and manual repository checks; I established the check baseline before implementation and reran the relevant checks afterward.
- What it produced: Format, lint, type-check, and the final production build pass. The test suite reproduces the same unrelated MongoMemoryServer baseline failure from before my change.
- Difficulties: The first post-build pnpm build failed while prerendering /, even though the changed integrations were admin pages.
- What changed: I compared the branch against a clean upstream/dev worktree, proved the base branch built successfully, and traced the regression to the shared Button's interaction with Radix Slot. I fixed the single-child behavior and reran the production build successfully.

---

## Implementation Notes

**2026-11-04**

I implemented the shared Button loading state, reusable CardSkeleton, and the two representative integrations from my plan. During validation, pnpm build failed while prerendering the home page even though the same build passed on a clean upstream/dev worktree. I traced the regression to the new Button spinner rendering: it gave Radix Slot two children for Button asChild, which broke landing-page links with React.Children.only. I changed the Button so non-loading and asChild cases receive the original single child, while a normal loading Button renders the spinner and content together. After the change, the full build passed and generated all 21 static pages.

**2026-11-05**

My first ship-gate run passed lint, type-check, and build but my own pr-precheck rejected the PR because test_evidence.md described the loading behavior without observable output. Since the authenticated admin routes redirected me to login locally, I did not bypass authentication to create screenshots. Instead, I rendered the real Button and CardSkeleton components with the repository's existing react-dom/server; all 12 verification checks passed, including disabled, aria-busy, spinner rendering, preserved Button content, the asChild regression guard, and the composed CardSkeleton structure. After adding that evidence, ship-gate and all five pr-precheck checks passed.

**2026-11-06**

The maintainer asked me to resolve conflicts against the updated dev branch and rerun formatting. Rebasing produced one conflict in the admin plugins page, where upstream had changed the imports while my PR changed the loading UI to CardSkeleton. I preserved the new upstream imports and my CardSkeleton integration, then fixed a missed import that lint and type-check caught. My force-push also replaced the maintainer's earlier merge commit, so I verified with Git that the latest upstream/dev was still fully included in my rebased branch and explained what happened in my maintainer reply. After the update, the PR monitor reported two passing checks and a mergeable PR.

**Toolkit**

- Tool: Claude Code with my CLAUDE.local.md, accepted plan.md, and the repository validation commands.
- What it produced: A four-file implementation adding the shared Button loading API, CardSkeleton, and representative integrations in the admin clubs and admin plugins pages.
- Difficulties: The initial shared Button implementation broke Button asChild during static prerendering because Radix Slot requires a single child.
- What changed: I changed the loading render path so asChild and non-loading Buttons receive their original single child, while normal loading Buttons render the spinner with their content. The final production build then passed.

---

## Pull Request

**PR Link:** https://github.com/NITRR-Official/CampusOS/pull/137

**PR Description:** The PR adds reusable loading behavior to the shared design system for issue #12. It adds a loading state to the shared Button with spinner, disabled, and aria-busy behavior, adds a reusable CardSkeleton, and integrates the shared loading components into the admin clubs and admin plugins pages. The PR also documents the repository checks, the existing pnpm test MongoMemoryServer failure, the rendered-DOM verification, and the parts of the broader loading-state issue that were intentionally left out of this bounded change.

**Maintainer Feedback:**

- 2026-11-05: None yet; PR opened and awaiting review.
- 2026-11-06: The maintainer asked me to resolve the conflicts in the PR, run the format check, and commit the updated branch. They also told me that the earlier Vercel issue had been fixed on their side.
- 2026-11-06: I rebased my branch onto the latest dev, resolved the conflict in the admin plugins page, reran formatting and the repository checks, and pushed the updated branch. During this process I accidentally replaced the maintainer's merge commit with my rebased history, so I checked that the latest upstream/dev was fully included and explained this in my reply. The PR monitor now reports the PR as mergeable with two passing checks and no review decision yet.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate with my pr-precheck; I used it as the final gate before opening the PR through Claude Code.
- What it produced: The final run passed CampusOS's lint, type-check, and build checks and all five of my pr-precheck checks: Plan fidelity, Description matches change, Evidence proves behavior, Diff reviewable, and Standards satisfied.
- Difficulties: The first gate run rejected the PR because my test evidence described the UI behavior without observable output, even though the repository checks passed.
- What changed: I added rendered-DOM verification for the shared Button and CardSkeleton, including the loading attributes, spinner, preserved label, asChild regression guard, and skeleton structure. The next ship-gate run passed, and I opened PR #137.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned more about working with shared React components in a real codebase, especially how a small change to a shared component can affect pages that are not directly related to the feature I am working on. The clearest example was the Button loading state: adding the spinner initially broke Radix Slot for Button asChild on the landing page. I also gained experience with Next.js loading patterns, reusable skeleton components, rendered-DOM verification with react-dom/server, and validating changes with lint, type-check, formatting, and production builds.

**Challenges Overcome**

The hardest part was keeping the change small while working in a repository that had changed significantly since I first claimed the issue. I had to decide what parts of the broad loading-state issue belonged in my PR, diagnose a production-build regression, and later rebase onto a much newer dev branch. The rebase created a conflict in the admin plugins page, and I initially kept the wrong import during the resolution. Lint and type-check caught that mistake, and I corrected it before updating the PR. I also accidentally replaced a maintainer merge commit during the force-push, so I verified that the latest upstream dev was still fully included and explained what happened to the maintainer.

**What I'd Do Differently Next Time**

I would check for upstream changes again immediately before starting implementation and again before rebasing, especially in a repository with active architecture work. I would also make my first test-evidence draft more observable instead of describing behavior only in prose. The ship-gate rejection showed me that rendered output or another concrete before/after check is much stronger evidence than simply saying the code now supports a behavior.

---

## Resources Used

- [CampusOS issue #12](https://github.com/NITRR-Official/CampusOS/issues/12) — defined the loading-state improvements and was the thread I used to ask the maintainer about scope and component placement.
- [CampusOS CONTRIBUTING.md](https://github.com/NITRR-Official/CampusOS/blob/dev/docs/contributing/CONTRIBUTING.md) — guided the branch, commit, formatting, lint, type-check, build, test, and PR requirements I followed.
- [CampusOS PR #137](https://github.com/NITRR-Official/CampusOS/pull/137) — maintainer feedback here guided the rebase, conflict resolution, formatting rerun, and updated branch.

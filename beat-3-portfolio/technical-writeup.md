# What a Loading-State PR Taught Me About Validating Shared React Components

## Overview

For my first real open-source contribution, I worked on CampusOS, a project for managing different parts of campus life. I picked issue #12, which asked for more consistent loading states across the application. I added a reusable loading state to the shared Button component, created a reusable `CardSkeleton`, and used both in existing admin pages. What looked like a small UI improvement ended up teaching me a lot about working in an unfamiliar codebase, testing shared components, responding to failures, and working with a maintainer. My pull request, [PR #137](https://github.com/NITRR-Official/CampusOS/pull/137), was eventually merged into the project.

## What happened

### Finding a smaller problem inside a broad issue

When I first read issue #12, the scope looked simple: add skeleton loaders, button loading states, reusable loading components, and page-level loading where needed. Once I explored the repository, I realized the situation was more complicated.

CampusOS already had a basic `Skeleton` component, but different pages were building their own loading layouts from it. The shared Button also did not have a standard loading state, so individual pages handled spinners and disabled states themselves. I also found UI components in both `frontend/components/ui` and `packages/design-system`, which made it unclear where new shared behavior should go.

Instead of trying to change loading behavior across the entire application, I narrowed my contribution. I added a loading API to the shared design-system Button, created a reusable `CardSkeleton`, replaced the manually built skeletons on one representative admin page, and moved one existing Approve action to the shared Button loading behavior. I intentionally left table skeletons, additional page-level loading files, and the duplicate component tree outside the PR.

### A shared Button change broke the home page

My first implementation looked correct for the admin pages I was changing. The Button displayed a spinner while loading, became disabled, and exposed `aria-busy`.

Then I ran the production build.

The build failed while prerendering `/`, even though the pages I had directly changed were admin pages. The important part of the failure pointed to React's single-child requirement:

```text
React.Children.only expected to receive a single React element child
```

I traced the problem to the shared Button. CampusOS uses Radix `Slot` when a Button is rendered with `asChild`. My first loading implementation always created two JSX child positions: a conditional spinner and the original children. Even when the spinner evaluated to `null`, React still gave the Slot a child structure that violated its single-child expectation.

That was a useful reminder that changing a shared component can affect much more than the page I am currently looking at. Landing-page links also used the same Button with `asChild`, which is why my loading-state change broke `/`.

I changed the rendering logic so non-loading Buttons and `asChild` Buttons receive their original child unchanged. Only a normal Button in the loading state renders the spinner together with its content. I reran the production build, and all 21 static pages generated successfully.

### My own PR gate rejected my evidence

Before opening the pull request, I used a tool I had built during the course called ship-gate. I configured it to run the repository's lint, type-check, and build commands and then run my PR precheck against my plan, diff, description, and test evidence.

The repository checks passed, but my own precheck rejected the PR:

```text
ship-gate: STOP. your own pr-precheck rejected feature/12-loading-states

Evidence proves behavior [fail]:
test_evidence.md before/after is prose about code state
with no observed output, DOM/aria-busy capture, or screenshot
```

This was interesting because the problem was no longer the implementation. The problem was that I was saying the Button worked without providing strong enough evidence that it actually behaved that way.

I tried accessing the affected admin pages locally, but they redirected me to the login page. I did not want to bypass authentication or modify production code just to create screenshots. Instead, I used the repository's existing `react-dom/server` installation to render the real Button and `CardSkeleton` components from a temporary local verification script.

The verification produced concrete results:

```text
Result: 12 passed, 0 failed

PASS  disabled present
PASS  aria-busy="true" present
PASS  data-loading="true" present
PASS  spinner rendered
PASS  label "Approve" preserved
```

I also verified the `asChild` case that had caused the earlier build regression and checked the rendered structure of `CardSkeleton`. The temporary verification files were deleted afterward and never became part of the PR.

After I added this evidence, I ran ship-gate again. This time the repository checks and all five of my PR prechecks passed.

### The repository changed while my PR was open

Opening the PR was not the end of the work. CampusOS was actively changing, and the maintainer later asked me to resolve conflicts with the updated `dev` branch and rerun formatting.

I rebased my branch onto the latest `dev`. Most of my changes applied automatically, but the admin plugins page conflicted because upstream had changed its imports while my PR had changed its loading UI to use `CardSkeleton`.

During my first conflict resolution, I preserved the `CardSkeleton` usage but accidentally kept the old `Skeleton` import. The repository checks caught the mistake immediately:

```text
'Skeleton' is defined but never used
'CardSkeleton' is not defined
```

I corrected the import, ran formatting again, and reran lint, type-check, and build. I also had to be careful with the branch history because my rebase and force-push replaced a merge commit the maintainer had added to my PR branch. I checked that the latest `upstream/dev` was still fully included in my history and explained what happened in my reply instead of hiding the mistake.

### The result

The updated pull request passed the project's checks and was merged on November 7, 2026.

```text
PR #137 — MERGED

Lint          SUCCESS
Type Check    SUCCESS
Format Check  SUCCESS
Test          SUCCESS
Build         SUCCESS
```

What started as a small loading-state improvement ended with reusable loading behavior becoming part of CampusOS. More importantly for me, the process changed how I think about contributing code. Getting the implementation to look correct was only one part of the job. I also had to prove the behavior, test the wider impact of a shared component, keep up with upstream changes, and communicate clearly when something went wrong.

## Limitations and what's next

My PR intentionally did not solve every loading-state inconsistency mentioned in issue #12. I did not add skeletons for the table-based admin pages, add loading files to more route groups, migrate the duplicate components under `frontend/components/ui`, or refactor every existing loading state. I kept the contribution small enough that I could understand and validate each change.

Another limitation was frontend testing. At the time I implemented the change, the frontend test script reported `No tests yet`, so I used rendered-DOM verification together with lint, type-check, formatting, and production builds instead of introducing a new testing framework just for this PR.

If I continued this work, I would first add automated component coverage for the shared Button. In particular, I would test both its loading behavior and the `asChild` case that broke during my first implementation. That regression showed me exactly why shared components benefit from focused automated tests. After that, I would gradually move more pages toward the shared loading patterns rather than trying to standardize the entire application in one large change.

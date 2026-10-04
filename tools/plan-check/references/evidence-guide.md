# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives:** In eval mode, read the Candidate plan's diagnosis and implementation explanation against the Issue section and the repro-evidence block, especially the reproduced trigger, observed output, and expected behavior. Use any code facts included in the bundle to distinguish observed behavior from an inferred cause. In live mode, read the diagnosis in `plan.md` against the live issue, the student's posted Unit 2 reproduction comment, and relevant code locations the plan cites.

**What good looks like:** The diagnosis explains the behavior the reproduction actually demonstrates and does not contradict its artifacts. A proposed cause may remain a hypothesis when the evidence does not prove it, but the plan must identify that uncertainty rather than present the inference as established fact.

## Scope

**Where it lives:** In eval mode, read the Candidate plan's in-scope changes, non-goals, files or areas to touch, and implementation approach against the Issue section and repro-evidence block. In live mode, read the scope, files to touch, and non-goals in `plan.md` against the live issue and the behavior established by the student's reproduction.

**What good looks like:** The proposed work is limited to changes needed for the reproduced issue, and the named files or areas form a coherent boundary for that outcome. Non-goals keep unrelated cleanup, redesigns, or feature work out, while the scope still includes work necessary to resolve the reproduced behavior.

## Executability

**Where it lives:** In eval mode, read the Candidate plan's files or code areas, implementation approach, and order of work against its diagnosis, the repro-evidence block, and any relevant code facts included in the package. In live mode, read the files and approach named in `plan.md` and inspect the relevant code in the student's fork when needed to verify that those locations contain the behavior the plan intends to change.

**What good looks like:** The plan identifies the code or behavior implicated by the diagnosis and explains what will change there and how that change addresses the reproduced failure. A contributor familiar with the repository can begin the implementation without guessing the material code location, intended behavior, or a major unresolved design choice; exact line-by-line edits are not required.

## Test plan

**Where it lives:** In eval mode, read the Candidate plan's test commands, inputs, and expected post-fix results against the repro-evidence block's original trigger and observed artifacts, plus the Issue section's expected behavior. In live mode, read the test plan in `plan.md` against the student's posted Unit 2 reproduction evidence and the relevant repository tests.

**What good looks like:** The test plan re-exercises the reproduced failure through the relevant code and states an observable result that should change after the fix. It should be possible to distinguish the fixed behavior from the original failure using the planned evidence; merely running a test suite or checking that a command exits successfully is not enough when the reported bug could remain.

## Honesty

**Where it lives:** In eval mode, read the Candidate plan's diagnosis, assumptions, risks, unknowns, and any stated certainty against the repro-evidence block and available code facts. If the package represents a plan revised during implementation, also read its Deviations section. In live mode, read these parts of `plan.md` against the student's reproduction evidence and relevant code in the fork.

**What good looks like:** Verified observations, hypotheses, and unresolved questions are distinguished from one another. Any uncertainty that could materially change the implementation or expected outcome is stated rather than presented as fact; when implementation differs from the posted plan, the Deviations section records what changed and why.

## Comms

**Where it lives:** In eval mode, read the Candidate plan comment and full plan against the Thread highlights and the repo-facts block, including maintainer guidance, contribution requirements, templates, and any stated AI-use or disclosure policy. In live mode, compare `comment.md` and `plan.md` with the live issue thread and the repository's contribution documentation and applicable policies.

**What good looks like:** The plan and comment respect material maintainer guidance and decisions already established in the thread and do not present a rejected or unresolved direction as settled. The outgoing comment accurately summarizes the contributor's own plan and follows applicable repository requirements, including AI-use disclosure when explicitly required; absence of such a requirement is not a failure.

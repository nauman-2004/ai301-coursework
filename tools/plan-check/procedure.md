# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue context first. Record the reported trigger, observed behavior, expected behavior, and any constraints or requested outcome stated by the issue author or maintainers.
2. Read the reproduction evidence next. Record the exact trigger that was run, the observed artifact or output, and what the evidence establishes about the failure. Keep observations separate from any explanation of the cause.
3. Read the repository facts and thread highlights. Record maintainer guidance, rejected or preferred approaches, contribution requirements, and any repository policy that could constrain the plan or its comment.
4. Read the complete plan. Record its diagnosis, scope and non-goals, files or code locations it proposes changing, implementation approach, test plan, and stated risks or unknowns.
5. Read the draft plan comment last. Record what it tells maintainers the contributor intends to change and test, and compare those claims with the full plan. Do not grade any check until all package parts have been read.

## Evidence gathering

1. For **Diagnosis grounded**, compare the plan's stated cause or investigation target with the issue's reported behavior and the reproduction artifacts. Record which reproduced facts support the diagnosis and any part of the diagnosis that remains an inference.
2. For **Scope bounded**, collect the plan's proposed changes, files to touch, and explicit non-goals. Compare them with the reproduced issue and record whether each proposed change contributes to that issue or introduces unrelated work.
3. For **Change targets cause**, identify the code or behavior the plan proposes modifying and compare it with the diagnosis and relevant code facts supplied by the package. Record the connection between the proposed edit and the reproduced failure.
4. For **Approach executable**, record the concrete code locations, behaviors, and implementation actions named by the plan. Note any material implementation decision that a contributor would still have to guess before beginning.
5. For **Test plan proves fix**, record the reproduction trigger and before-fix result, then record the plan's post-fix commands, inputs, and expected results. Determine whether the planned checks would visibly distinguish the fixed behavior from the reproduced failure.
6. For **Unknowns honest**, collect every stated risk, uncertainty, assumption, or hypothesis from the plan. Compare those statements with the reproduction and code evidence and record any material uncertainty the plan presents as settled fact or leaves unstated.
7. For **Thread and repo aligned**, collect relevant maintainer comments, thread decisions, repository contribution requirements, and applicable policies. Compare them with both the full plan and the draft plan comment and record any conflict or unmet requirement.
8. In live mode, gather issue, thread, and repository-side evidence only from the locations identified in `references/evidence-guide.md`. In eval mode, use only the package bundle and do not fetch outside information.

## Check execution

1. Grade the checks in rubric order: **Diagnosis grounded**, **Scope bounded**, **Change targets cause**, **Approach executable**, **Test plan proves fix**, **Unknowns honest**, then **Thread and repo aligned**.
2. For each check, use only the evidence gathered for that check and apply the pass and fail conditions written in `rubric.md`. Do not substitute a general impression of whether the plan seems good.
3. Grade `pass` when the available evidence satisfies the check's pass condition and does not meet its fail condition.
4. Grade `fail` when the evidence demonstrates a condition that the check explicitly says should fail.
5. Grade `unclear` when evidence required to decide the check is genuinely absent or insufficient. Do not use `unclear` merely because the diagnosis contains an explicitly identified hypothesis or risk; evaluate those under the check's stated condition.
6. When one fact affects multiple checks, reuse that recorded fact rather than reinterpreting the whole package differently for each check. For example, an unsupported root-cause claim may affect both **Diagnosis grounded** and **Unknowns honest**, but each check must still be graded by its own pass condition.
7. Record one concise evidence fact or quote that directly explains each grade. For a failure or unclear grade, use the missing, contradictory, or unsupported evidence that caused that result.

## Verdict assembly

1. After all checks have been graded, apply the verdict rule from `rubric.md` exactly as written.
2. Return `accept` only when every required check is graded `pass`.
3. Return `reject` when any required check is graded `fail` or `unclear`. An `unclear` required check therefore holds the plan just like a failed required check.
4. Preferred checks, if any are later added to the rubric, never change the final verdict.
5. In the readable summary, report every check's grade and the evidence fact or quote that decided it. For a rejected package, make the failed or unclear required checks explicit so the contributor knows what prevents the plan from being ready.
6. In the final JSON, include every rubric check in rubric order with its grade and one concise evidence statement. Set `"verdict"` to only `"accept"` or `"reject"`, and ensure the JSON block is the final content in the output.

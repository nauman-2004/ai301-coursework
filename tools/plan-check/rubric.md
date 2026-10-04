# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis grounded | The plan's diagnosis and proposed cause read against the reproduced behavior and artifacts, using the Diagnosis section of `references/evidence-guide.md` | Pass if the diagnosis is consistent with the reproduced evidence and identifies a plausible cause or investigation target without claiming more certainty than the evidence supports. Fail if it contradicts the reproduction, assumes an unsupported cause, or diagnoses a different problem from the one reproduced. | required |
| Scope bounded | The plan's stated scope, files to touch, proposed changes, and explicit non-goals read against the issue and reproduced behavior, using the Scope section of `references/evidence-guide.md` | Pass if the proposed work is bounded to changes needed to address the reproduced issue and the stated non-goals keep unrelated improvements out. Fail if the plan expands into unrelated work, leaves the implementation boundary too vague to know what will change, or omits necessary work while claiming the issue will be resolved. | required |
| Change targets cause | The plan's diagnosis, files to touch, and implementation approach read against the reproduced evidence and relevant code facts, using the Implementation target section of `references/evidence-guide.md` | Pass if the proposed change acts on the code or behavior implicated by the evidence and explains how that change addresses the reproduced failure. Fail if it only masks the observed symptom, changes an unrelated layer, or proposes a workaround without connecting it to the diagnosed cause. | required |
| Approach executable | The plan's files to touch and implementation approach, read against the diagnosis and scope, using the Execution detail section of `references/evidence-guide.md` | Pass if a contributor familiar with the repository could begin implementing the bounded change from the plan without having to guess the material code location, behavior to change, or intended result. Fail if the approach is too vague to start, leaves a material implementation decision unresolved while presenting the plan as ready, or does not connect the proposed edits to the diagnosed problem. | required |
| Test plan proves fix | The plan's test commands, inputs, and expected post-fix outcomes read against the Unit 2 reproduction evidence and the issue's expected behavior, using the Test plan section of `references/evidence-guide.md` | Pass if the planned checks exercise the reproduced failure through the relevant code and name an observable post-fix result that would distinguish a real fix from the original behavior. Fail if the tests do not cover the reproduced trigger, only check that code runs, or could pass while the reported bug remains. | required |
| Unknowns honest | The plan's diagnosis, risks, unknowns, and implementation claims read against the reproduction evidence and available code facts, using the Risks and unknowns section of `references/evidence-guide.md` | Pass if material uncertainties or risks are stated honestly and the plan distinguishes verified facts from hypotheses that still need validation during implementation. Pass when no material unknown remains and the evidence supports that certainty. Fail if an unresolved assumption could change the implementation or outcome but is presented as established fact or omitted. | required |
| Thread and repo aligned | The draft plan comment and plan read against the issue thread highlights, maintainer guidance, repo-facts contribution requirements, and applicable repository policies, using the Thread and conventions section of `references/evidence-guide.md` | Pass if the proposed plan respects relevant maintainer guidance and decisions already established in the thread, and the outgoing comment follows applicable repository requirements. Fail if it contradicts or ignores material thread guidance, presents an already-rejected direction as settled, or violates a stated contribution or communication requirement. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks do not affect the verdict.

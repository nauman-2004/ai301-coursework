# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The Candidate repro report's environment record, read against the Issue section and the repo-facts bug-report requirements using the Environment section of `references/evidence-guide.md` | Pass if the repro report records the issue-relevant environment details needed to understand the reproduction attempt, and any meaningful difference from the issue's target environment is explicitly stated. | required |
| Steps followable | The Candidate repro report's setup, inputs, commands, configuration, and trigger, using the Steps section of `references/evidence-guide.md` | Pass if a stranger starting from the recorded environment can follow the provided setup, inputs, commands, and actions to reach the attempted trigger without having to guess any material step. | required |
| Behavior matches issue | The Issue section's trigger and described behavior compared with the Candidate repro report's commands and artifacts, using the Behavior shown section of `references/evidence-guide.md` | Pass if the report's artifacts show the result of attempting the issue's relevant trigger and provide enough evidence to determine whether the issue's described behavior occurred. A different input, generic error, adjacent failure, or unrelated non-zero exit does not count as showing the issue's behavior. | required |
| Outcome honest | The Candidate claim comment and Candidate repro report's stated conclusions compared with the commands and artifacts, using the Honesty section of `references/evidence-guide.md` | Pass if the comments state only what the evidence supports: reproduced when the artifacts demonstrate the target behavior, cannot reproduce or partial when they do not, and no unsupported root cause, fix, or certainty is claimed. | required |
| Repo conventions | The Candidate claim comment and Candidate repro report compared with the Issue section and the repo-facts bug-report template and contribution policy, using the Comms section of `references/evidence-guide.md` | Pass if the claim is specific to the issue and honestly states the investigation or report the contributor will do, and the outgoing comments satisfy applicable repository requirements, including AI-use disclosure when the repository explicitly requires it. Absence of an AI policy or disclosure requirement does not fail the check. | required |

## Verdict rule

Accept if every applicable required check passes. Reject if any applicable required check fails or is unclear. In live claim-only mode, checks that require the repro report are not yet applicable and do not affect the verdict, as defined in SKILL.md. Preferred checks do not affect the verdict.

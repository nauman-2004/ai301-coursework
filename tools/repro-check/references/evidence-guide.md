# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**Where it lives:** In eval mode, read the environment record in the Candidate repro report against the Issue section and the repo-facts bug-report requirements. Look for relevant software/runtime versions, installation method, operating system or platform, and dependency versions when the issue or thread makes them relevant. In live mode, use the issue thread and repo's bug-report or contribution docs to determine which environment details matter, then read those details from the student's draft repro comment.

**What good looks like:** The report identifies enough of the environment for a stranger to understand the conditions that produced the result and includes environment details specifically relevant to the issue. The environment does not have to exactly match the original report, but meaningful differences such as testing a different software version or platform are stated rather than hidden.

## Steps

**Where it lives:** In eval mode, read the commands, setup actions, inputs, configuration, and trigger described in the Candidate repro report, using the Issue section only to understand what behavior is being attempted. In live mode, read the reproduction procedure included directly in the student's draft repro comment.

**What good looks like:** Starting from the recorded environment, a stranger can follow the report from the required starting state to the attempted trigger without guessing a material command, input, configuration, or action. The report may reuse setup or input from the issue if it identifies it precisely enough to reproduce, but simply saying "same as above" or claiming the issue was reproduced without giving a followable procedure is not sufficient.

## Behavior shown

**Where it lives:** In eval mode, compare the Issue section's described trigger, expected behavior, and reported failure with the commands and artifacts in the Candidate repro report, including output excerpts, logs, errors, exit codes, or screenshots. In live mode, compare the issue thread with the evidence included directly in the student's draft repro comment.

**What good looks like:** The artifacts show the same behavior the issue describes under the relevant trigger, or they clearly show that an honest reproduction attempt did not produce that behavior. A different input, generic error, adjacent failure, or non-zero exit code does not prove the issue unless it matches the behavior being investigated.

## Honesty

**Where it lives:** In eval mode, compare the Candidate repro report's stated outcome and conclusions with its commands and artifacts, read against the behavior described in the Issue section. Also compare the Candidate claim comment with what had actually been established at that point. In live mode, compare each factual claim in the student's draft comments with the evidence included in those drafts and the issue thread.

**What good looks like:** The report states only what its evidence supports. A reproduced result is called reproduced only when the artifacts demonstrate the issue's target behavior, and a failed or partial reproduction is reported as such rather than turned into a success claim. The report may state an evidenced cannot-reproduce result and still be ready; it must not claim a root cause, fix, or certainty that the shown evidence does not establish.

## Comms

**Where it lives:** In eval mode, compare the Candidate claim comment and Candidate repro report with the Issue section and the repo-facts bug-report template and contribution policy, including any stated AI-use or disclosure requirements. In live mode, compare the student's draft comments with the live issue, the repository's contribution and bug-report docs, templates, and any AI-use policy.

**What good looks like:** The claim is specific to the issue and honestly states what the contributor will investigate or report rather than making unsupported promises. The outgoing comments follow any applicable repository requirements, including required AI-use disclosure. If the repository has no stated AI disclosure requirement, silence about AI is not a failure.

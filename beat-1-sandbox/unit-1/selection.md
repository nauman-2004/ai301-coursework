# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/59

**Verdict output**

All four required checks pass. Summary and machine-readable result below.

Issue #59, "Faithfulness checker scores claims unsupported when the context uses different words" (labels: bug, rag, tier-1, opened by collaborator Aburke225 on 2026-09-10).

- Issue unclaimed: pass. No assignees, zero comments, and no linked or cross-referenced PRs in the timeline. Only label events exist on the issue.
- Repo in use: pass. Repo is not archived. Three of the last five default-branch commits landed on 2026-09-16, four days before today.
- Maintainer alive: pass. All five recent commits are authored by Andrew Burke (Aburke225, a human User account with the Collaborator badge). The same maintainer replied on issues #52 and #43 on 2026-09-16, six days after they were opened.
- Scope fits newcomer: pass. The body names one function, one file, and one failing test. There is no umbrella language, no open design debate, no core-internals warning, and no history of abandoned attempts.

Fit notes, from the profile in scope.md. This is Python in the RAG subsystem, so it gives direct exposure to an AI evaluation concept (faithfulness scoring) while staying within familiar tooling. The repro is a single pytest command, which matches the stated preference for a reproducible way to verify the fix.

Two practical notes outside the rubric. The repo's docs/CONTRIBUTING.md has no AI-use policy, so the fifth surface passes by silence. It does say seeded bugs carry a strict xfail marker on their test, and removing that marker is part of the fix, so expect to touch the test file as well.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/59",
  "checks": [
    {"name": "Issue unclaimed", "grade": "pass",
     "evidence": "assignees: []; comments: 0; closedByPullRequestsReferences and cross-reference timeline events: none (only three LabeledEvents on 2026-09-10)"},
    {"name": "Repo in use", "grade": "pass",
     "evidence": "archived: false; last 5 main commits dated 2026-09-16 (x3) and 2026-08-24 (x2), all within 90 days of 2026-09-20"},
    {"name": "Maintainer alive", "grade": "pass",
     "evidence": "All 5 recent commits authored by human user Aburke225 (COLLABORATOR) within 90 days; Aburke225 also replied on issues #52 and #43 on 2026-09-16"},
    {"name": "Scope fits newcomer", "grade": "pass",
     "evidence": "Body names one deliverable: fix `_is_supported()` in faithfulness_checker.py so support does not depend on shared wording, verified by `pytest tests/unit/test_faithfulness_checker.py -q`; no umbrella items, design debate, core-internals note, or abandoned PRs"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

The first smoke-test attempt on issue-01 errored because my Claude OAuth session had expired, so it produced no agreement score. After re-authenticating, my scored runs in order were: 0/1, 0/1, 1/1, 16/20, 2/4, 2/2, 2/2, 20/20, and 19/20. The final 19/20 was the complete run saved to `eval-run.txt`.

**Issue analysis**

I analyzed `issue-20`. In my saved eval run, my rubric decided `accept`, while the gold label was `reject`. My scope check looks for a bounded deliverable and rejects unresolved design or product decisions that prevent knowing what to implement. The issue gave a concrete success condition: add a logo tool that lets users place, resize, move, and correctly export a company logo, so the grader interpreted that as a bounded deliverable. However, the issue also leaves the logo asset TBD and says additional app wiring may be needed. The gold label treated those unresolved details as an unsettled product decision, while my run treated the stated user-visible outcome as enough to pass the scope check.

**Check rationale**

I used this current check:

> | Scope fits newcomer | The Issue body and Comments section, plus "linked PRs" under Repo facts; look for the requested deliverable, umbrella/tracking language, whether listed ideas are required work or only suggestions, unresolved design/product decisions, maintainer statements about core internals, and repeated abandoned attempts. | Pass if the issue defines a bounded deliverable or coordinated changes serving one defined outcome. Suggestions or optional approaches do not count as required work unless the issue or a maintainer says they are required. Fail if the issue is an umbrella/tracking issue containing independent work items, unresolved design or product decisions prevent knowing what to implement, a maintainer says the work requires changes to core internals, or the history shows repeated abandoned attempts such as multiple closed unmerged PRs or multiple contributors claiming the issue and failing to complete it. | required |

I kept scope as a required check because an issue can be active and unclaimed but still be a poor first contribution if the actual work is not sufficiently bounded. I revised the check after the eval showed that my original wording, "one bounded piece of work," was too restrictive: issue-01 had several documentation changes, but they all served one defined outcome. I therefore allowed coordinated changes serving one outcome. I also added linked-PR and attempt history after issue-15 showed that repeated abandoned attempts can reveal hidden difficulty, and clarified that optional suggestions are not automatically required work after testing issue-19.

**Trade-offs**

The trade-off in my scope check is that allowing a clearly stated deliverable to pass can sometimes accept an issue whose implementation still hides an unresolved product decision. `issue-20` demonstrates this: my saved run accepted it because the desired logo-tool behavior was concrete, while the gold label rejected it because details such as the logo asset were still undecided. I accept this trade-off because making the rule stricter could again reject bounded issues like `issue-01`, where several coordinated changes still serve one clearly defined outcome.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I chose issue #59 because it is related to RAG and AI/ML, which is an area I am interested in learning more about. It is also a tier-1 issue with a focused scope, so it seems manageable within the time available.

2. The verdict correctly identified that the issue is unclaimed, the repository and maintainer are active, and the work has a bounded scope with a specific function and failing test. Beyond the rubric, I also weighed my personal interest in AI/RAG heavily because I want this contribution to help me gain more experience with AI-related application logic.

3. I expect the difficulty of claiming and getting started on the issue to be moderate. I have some experience working with AI/RAG concepts, and the named function and failing test give me a clear starting point, but I will still need to understand how this project's faithfulness checker works before deciding on the fix.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

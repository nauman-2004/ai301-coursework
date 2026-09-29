# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I am a computer science student and a newer open-source contributor with software development experience. When I comment on an issue, I am there to investigate the reported behavior, reproduce it in my own environment, and share evidence clearly before attempting a fix. I may be learning parts of the codebase as I work, so I will be clear about what I have verified and what I have not.

## Rules I write by

### Rule: Do not claim results before I verify them

Before I reproduce or investigate something, I describe what I plan to check instead of writing as if I already know the result.

- Wrong: "I reproduced this bug and know what is causing it."
- Right: "I plan to reproduce this issue locally, investigate the reported behavior, and report back what I find."

### Rule: Name the specific behavior I am investigating

My comments should refer to the actual behavior in the issue instead of using a generic message that could apply to any issue.

- Wrong: "I would like to work on this issue. Please assign it to me."
- Right: "I plan to investigate why the faithfulness checker marks supported claims as unsupported when the context uses different wording."

### Rule: Do not promise a fix or deadline

I can commit to investigating and reporting what I find, but I should not promise that I will fix the issue or finish it by a specific date before I understand the problem.

- Wrong: "I'll fix this and have a PR ready by tomorrow."
- Right: "I'll investigate the issue and report back with what I find."

## Things I never post

- Claims that I reproduced or understood something before I verified it.
- Promises that I will fix an issue or finish by a specific date.
- Generic claim comments that do not mention the issue's actual behavior.
- Conclusions that go beyond what my commands, logs, tests, or other evidence show.
- "Same as above" reproduction comments that rely on someone else's evidence instead of showing my own.

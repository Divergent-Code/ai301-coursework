# Rubric: is this plan ready to post and build from?

## Inputs

- **contributor-workflow:** AI-assisted. This is a fact about this course's contribution workflow, not something a package states. Change this line if the contributor works without AI assistance; the checks below read it by name.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's stated cause, read against every behavior the repro evidence pins down (the failing step, expected vs actual, controls, timings, conditions) | Principle: the stated cause explains what the repro evidence shows. Pass if the cause accounts for every behavior the repro evidence pins down and contradicts none of it. A cause taken from the issue or the thread passes only if the repro evidence supports it. Fail if the evidence shows a behavior the cause cannot explain. | required |
| scope-bounded | The plan's in-scope and out-of-scope statements and the changes it lists, read against the cause it diagnoses | Principle: the plan is one bounded change aimed at the diagnosed cause. Pass if the plan says what is in and out, and everything in scope serves the diagnosed cause. Fail if it adds unrelated work, or changes more than the cause needs (a rewrite where a targeted change would do). | required |
| executable | The plan's named files or code areas and its approach or order of work | Principle: a stranger could start the work without asking the author anything. Pass if the plan names the files or code areas it will change and the concrete first change there. Fail if the location is unnamed or vague ("poke around", "somewhere in the editor code"). | required |
| test-flips-repro | The plan's test plan, read against the repro evidence's steps and its expected and actual results | Principle: the test proves the reproduced failure is gone. Pass if the test plan re-runs the repro steps, or an equivalent check through the real code, and names the observable result that must change from the repro's actual to its expected. Fail if it only says the full suite passes or nothing regresses, or only says it will work. Extra regression checks alongside a repro check are fine. | required |
| certainty-earned | Every statement of cause, effort, or outcome in the plan and the plan comment, read against the repro evidence and the thread | Principle: the plan is only as certain as its evidence. Fail if the plan or comment states as settled something the evidence does not establish (a cause stated as found when the evidence only fits it, an effort or time estimate stated as fact, a fix promised as certain), or leaves out an unknown the evidence plainly leaves open. Pass if remaining unknowns and risks are named as unknowns. | required |
| thread-aware | The thread highlights (each commenter's role, what they said about cause, scope, or status, and the issue's labels), read against the plan comment and the plan | Principle: the comment answers what the thread actually says. Pass if the comment and plan engage with every thread signal that bears on the plan (an owner's or collaborator's analysis, a "known problem" or "fix is hard" note, a stale label, another contributor's claim or theory) and contradict none without saying why. Fail if the comment ignores a maintainer signal that changes the plan, or repeats a thread claim the repro evidence contradicts. A thread with no such signal passes. | required |
| follows-conventions | The repo-facts block (contribution policy, review process, templates, any AI-use policy) and the **contributor-workflow** input, read against the plan comment | Principle: the comment works within the repo's stated rules. A requirement applies only where the policy says it applies. When contributor-workflow is AI-assisted, a requirement to disclose AI use that applies to comments passes only if the disclosure appears in the comment. A requirement about how text was produced ("written in your own words") passes unless something contradicts it. A stated review or contribution process (for example, outside PRs reviewed selectively, or an accepted issue required first) passes only if the comment works within it or acknowledges it. A policy with no ask that bears on the comment passes. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Unclear counts as fail. Preferred checks never change the verdict.

## Notes on heuristics

- **cause-grounded:** "every behavior the repro evidence pins down" is the test; the judgment is in deciding whether a cause could produce a given behavior, which needs a reasonable reading of the evidence, not proof.
- **certainty-earned:** the line between a cause that is "found" and one that "fits the evidence" is a judgment call. The test is whether the evidence rules out the obvious alternatives.
- **thread-aware:** "bears on the plan" is a judgment call. A signal bears on the plan if acting on it would change the cause, the scope, or what the comment should say.
- **follows-conventions:** the AI-disclosure part reads the contributor-workflow input above instead of assuming it. Sorting a policy sentence into "state something" versus "how it was produced" is a judgment call when it mixes both.

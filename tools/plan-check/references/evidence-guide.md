# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- **Where it lives.** Eval bundle: the cause stated in the candidate plan (often under "Cause" or "Diagnosis"), read against the "Repro evidence" block's steps, expected and actual results, controls, and timings. Live mode: the cause in the student's draft `plan.md`, read against the student's own posted repro comment on the issue.
- **What good looks like.** Every behavior the repro evidence pins down is something the stated cause would produce, including the controls (what makes the failure go away). A cause that matches the issue's wording but cannot explain a control or a timing in the repro evidence is not grounded. A cause borrowed from a commenter is grounded only when the repro evidence supports it.

## Scope

- **Where it lives.** Eval bundle: the candidate plan's in-scope and out-of-scope lines and its list of changes. Live mode: the same parts of the draft `plan.md`.
- **What good looks like.** One change aimed at the diagnosed cause, with what is left out named. Every listed change serves that cause. Unrelated cleanups, refactors, or features added "while I'm here", or a rewrite where a targeted change would do, are scope creep.

## Executability

- **Where it lives.** Eval bundle: the files, functions, or code areas the candidate plan names, and its approach or numbered changes. Live mode: the draft `plan.md`'s files and approach, checked against the repository's actual file layout.
- **What good looks like.** A stranger can open the named file or area and know the first concrete change to make there. "Poke around the editor components" or "somewhere in the code" gives them nowhere to start.

## Test plan

- **Where it lives.** Eval bundle: the candidate plan's test plan, read against the "Repro evidence" block's steps and its expected and actual results. Live mode: the draft `plan.md`'s test plan, read against the student's posted repro comment.
- **What good looks like.** The test re-runs the reproduction (or an equivalent check through the real code) and names the result that must change from the repro's actual to its expected. "The full suite passes" or "it works after the fix" proves nothing about the reported failure on its own.

## Honesty

- **Where it lives.** Eval bundle: every statement of cause, effort, or outcome in the candidate plan and plan comment, read against the repro evidence and the thread. Live mode: the same, in the draft `plan.md` and comment, plus the plan's risks and unknowns and, after the build, its Deviations section.
- **What good looks like.** Claims are as strong as the evidence: "the evidence fits X" where it only fits, "I traced this to X" only where the evidence rules out the alternatives. Unknowns the evidence leaves open are named. Effort guesses ("should be doable in a few evenings") and promised outcomes are warning signs. After a build, a deviation is recorded in the plan, not only in the diff.

## Comms

- **Where it lives.** Eval bundle: the "Thread highlights" section (each commenter's role and what they said, plus the issue's labels) and the "Repo facts" block (contribution policy, review process, templates, AI-use policy), each read against the candidate plan comment. Live mode: the issue's live comment thread, including other contributors' comments, and the repo's `CONTRIBUTING.md`, templates, and any AI policy file, read against the draft comment.
- **What good looks like.** Thread-aware: the comment builds on or explicitly answers each signal that bears on the plan, such as an owner's analysis, a "known problem" note, a stale label, or another contributor's claim, and does not contradict one without saying why. Convention-aware: the comment works within the stated review and contribution process and includes any disclosure the policy requires of comments (reading the rubric's contributor-workflow input). Boilerplate that could be posted on any issue is neither.

# Procedure: how this skill grades a plan package

## Read order

Read the parts in this order, before grading any check. The plan is judged against the evidence, so the evidence is read first; reading the plan first anchors on its story.

1. Read the rubric's **Inputs** section and note the value of `contributor-workflow`.
2. Read the **repo facts** (live: the repo's `CONTRIBUTING.md`, templates, and any AI policy file). Note every stated ask that bears on comments or contributions: review process, templates, disclosure rules, and where each one applies.
3. Read the **issue**. Note the reported behavior in one line.
4. Read the **repro evidence** (live: the student's own posted repro comment on the issue). Note each behavior it pins down: the failing step, expected vs actual, every control, and any timing or condition.
5. Read the **thread highlights** (live: the full comment thread). For each comment, note the commenter's role and whether it says anything about cause, scope, or status. Note the issue's labels.
6. Read the **candidate plan** (live: `plan.md`). Note its stated cause, in-scope and out-of-scope lines, named files or areas, first change, test plan, and any stated unknowns or risks.
7. Read the **candidate plan comment** (live: the draft comment). Note its claims, promises, disclosures, and any references to the thread.

## Evidence gathering

For each check, pull the evidence from the notes made above, using the locations in `references/evidence-guide.md`. Record each piece as a short quote, not a paraphrase.

1. **cause-grounded:** the plan's cause (note 6) and the list of pinned-down behaviors (note 4).
2. **scope-bounded:** the in-scope and out-of-scope lines and listed changes (note 6), and the cause (note 6).
3. **executable:** the named files or areas and the first change (note 6).
4. **test-flips-repro:** the test plan (note 6), and the repro's steps with its expected and actual results (note 4).
5. **certainty-earned:** every statement of cause, effort, or outcome in the plan and comment (notes 6 and 7), and the evidence that would support each (notes 4 and 5).
6. **thread-aware:** each thread signal that bears on the plan (note 5), and what the comment and plan say about it (notes 6 and 7).
7. **follows-conventions:** each repo ask and where it applies (note 2), the `contributor-workflow` value (note 1), and the comment's text (note 7).

## Check execution

1. Run the checks in this order: cause-grounded, scope-bounded, executable, test-flips-repro, certainty-earned, thread-aware, follows-conventions. The diagnosis comes first because scope and the test plan are judged against the diagnosed cause.
2. For each check, compare the gathered quotes with the check's pass condition in `rubric.md`, and grade it `pass`, `fail`, or `unclear` with one line of evidence.
3. If the **plan or comment** is missing the part a check reads (for example, no test plan or no named files), grade `fail`: a missing part is the plan's own gap.
4. Grade `unclear` only when **package-side** evidence the check needs is genuinely absent (for example, no repro evidence to compare a cause against). Say which evidence is missing.
5. A check may be graded from the gathered quotes alone. Re-read a part of the package only if the quotes for that check do not settle the grade.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes; any `fail` or `unclear` on a required check makes the verdict reject.
2. For a reject, name the first failing check in execution order as the deciding check, and quote its evidence line in the summary.
3. Output a short summary with one line per check, then the fenced JSON block from `SKILL.md` as the last thing in the output.

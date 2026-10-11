# Procedure: how this tool grades a PR package

## Read order

Read the parts in this order before grading any check. The plan is read before the diff so the diff is judged against the plan, and the description is read after the diff so its claims are checked against what is actually there.

1. Read the rubric's **Inputs** section and note the value of `contributor-workflow`.
2. Read the **repo facts** (live: `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, and any AI policy file). List every section the PR template asks for, every contributing ask, and every disclosure requirement, with where each applies.
3. Read the **issue** and its **thread**. Note the reported behavior in one line and any explicit maintainer direction.
4. Read the **plan context** (live: `plan.md`). Note its in-scope and out-of-scope statements, named files, each planned change, each failure mode or repro step in its test plan, and every recorded deviation.
5. Read the **diff** and **commit list** (live: `git diff main...HEAD` and `git log main..HEAD`). For each changed file and hunk, note what it changes in one line.
6. Read the **title and description** (live: the draft file; its first line is the title). Note every claim about what changed, what was left out, and how faithful the PR is to the plan, plus every template section and disclosure it contains.
7. Read the **test evidence** (live: the evidence file). For each command, note what it shows and whether its output is visible.

## Evidence gathering

For each check, pull the evidence from the notes above, using the locations in `references/evidence-guide.md`. Record each piece as a short quote, not a paraphrase.

1. **diff-within-plan:** the per-hunk notes (note 5), set against the plan's scope, files, planned changes, and deviations (note 4), plus any omission the description discloses (note 6).
2. **diff-clean:** the per-hunk notes and commit list (note 5).
3. **description-matches-diff:** each claim in the title and description (note 6), set against the per-hunk notes (note 5).
4. **tests-shown:** each test-plan step (note 4), set against the evidence notes (note 7), plus the repo's own checks or suite in the evidence.
5. **meets-repo-asks:** the list of asks and requirements (note 2), the maintainer direction (note 3), the `contributor-workflow` value (note 1), and what the title and description contain (note 6).

## Check execution

1. Run the checks in this order: diff-within-plan, diff-clean, description-matches-diff, tests-shown, meets-repo-asks. The diff is graded against the plan first, because the description's claims and the test evidence are judged against what the diff actually contains.
2. For each check, compare the gathered quotes with the check's pass condition in `rubric.md`, and grade it `pass`, `fail`, or `unclear` with one line of evidence.
3. Apply the honest-shortfall wording in each pass condition exactly: a disclosed omission, a stated shortfall, or an explained failing check is graded as that condition says, not as a fail by default.
4. If the **PR** is missing the part a check reads (for example, no test evidence at all, or no description), grade `fail`: a missing part is the PR's own gap.
5. Grade `unclear` only when **package-side** evidence the check needs is genuinely absent (for example, no plan context to compare the diff against). Say which evidence is missing.
6. A check may be graded from the gathered quotes alone. Re-read a part of the package only if the quotes for that check do not settle the grade.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes; any `fail` or `unclear` on a required check makes the verdict reject.
2. For a reject, name the first failing check in execution order as the deciding check, and quote its evidence line in the summary.
3. Output a short summary with one line per check (and, in live mode, any voice-guide notes), then the fenced JSON block from `SKILL.md` as the last thing in the output.

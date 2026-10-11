# Evidence guide: where evidence lives in a PR package

This file says where each kind of evidence lives and what it looks like. The rules that grade it are in `rubric.md`, and each section names the check that judges it.

## Plan fidelity (harness category: silent-drift)

- **Where it lives.** Eval bundle: the "Plan context" block's in-scope and not-in-scope statements, named files, planned steps, and any deviation notes, read against the candidate PR's diff and commits; and the candidate PR's title and description, read against the same diff. Live mode: `plan.md` (including its Deviations section) and the draft PR text, read against `git diff main...HEAD`.
- **What good looks like.** Each file the diff touches can be matched to a line in the plan's scope, its named files, or a numbered deviation note. Each planned change can be found in the diff, or in a sentence of the description saying it was left out. Good places to look: the `+++ b/` file headers in the diff, the plan's Files list, and its Deviations section.
- **Judged by:** `diff-within-plan` and `description-matches-diff` in `rubric.md`.

## Test evidence (harness category: not-tested)

- **Where it lives.** Eval bundle: the candidate PR's "Test evidence" section, read against the plan context's test plan (each failure mode or repro step it names) and its repro evidence. Live mode: the student's captured test-evidence file, read against `plan.md`'s test plan and the student's posted repro.
- **What good looks like.** A fenced block holding the command as the plan names it and the output it printed after the change, an `exit code:` line, or a test count such as `Tests 3 passed (3)`. Other steps appear as one-line results that name the command or control and its outcome. A skipped step or failing check appears next to a sentence giving the reason.
- **Judged by:** `tests-shown` in `rubric.md` (clauses P1–P2, E1–E2, F1–F3).

## Diff quality (harness category: unreviewable)

- **Where it lives.** Eval bundle: the candidate PR's "Diff" (the unified diff) and "Commits" list. Live mode: `git diff main...HEAD` and `git log main..HEAD`.
- **What good looks like.** A short file list where each hunk's purpose is clear from its own lines. Signs worth quoting when they appear: `console.log` or `print` lines, blocks of commented-out code, hunks that only change whitespace, a lockfile or manifest change the description never mentions, files like `plan.md` or `notes.txt`, and commit messages like "gofmt" or "cleanup" next to a feature change.
- **Judged by:** `diff-clean` in `rubric.md`.

## Standards and comms (harness category: standards-wall)

- **Where it lives.** Eval bundle: the "Repo facts" block's pull-request line (the template's sections and checklist, contributing asks) and contribution-policy line (including any AI-use rule), plus the thread highlights for explicit maintainer direction, read against the candidate PR's title and description. Live mode: the repo's `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, any AI policy file, and the live thread, read against the draft PR text.
- **What good looks like.** The description carries each of the template's headings, each with its own text under it rather than the template's placeholder comments. It has an issue reference such as `Fixes #123` or `Closes #123`, and any disclosure the repo's policy asks for, as a sentence a reader can find. Listing the template's sections next to the description's headings makes gaps easy to spot.
- **Judged by:** `meets-repo-asks` in `rubric.md` (clauses A1–A2 cover AI-use disclosure).

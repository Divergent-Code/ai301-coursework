# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- **Where it lives.** Eval bundle: the "Plan context" block's in-scope and not-in-scope statements, named files, planned steps, and any deviation notes, read against the candidate PR's diff and commits; and the candidate PR's title and description, read against the same diff. Live mode: `plan.md` (including its Deviations section) and the draft PR text, read against `git diff main...HEAD`.
- **What good looks like.** Every changed file and hunk is part of a planned change or a recorded deviation, and every planned change appears in the diff or is disclosed as left out. Silent drift runs both ways: a diff that does more than the plan (an unrelated refactor, a dependency bump) or less (a planned file never touched) with no note. The description's claims belong here too: "implements the plan exactly" beside an unrelated hunk is drift, not a comms problem.

## Test evidence (harness category: not-tested)

- **Where it lives.** Eval bundle: the candidate PR's "Test evidence" section, read against the plan context's test plan (each failure mode or repro step it names) and its repro evidence. Live mode: the student's captured test-evidence file, read against `plan.md`'s test plan and the student's posted repro.
- **What good looks like.** The plan's main reproduction is re-run exactly as the plan names it, with an observable result after the change: pasted output, an exit code, a test count. Every other named step (controls, extra failure modes, the repo's own checks or suite) is reported as a specific result someone could re-run, such as "`cargo test` passes" or "the control is unchanged". Not tested looks like: the main reproduction claimed only in prose ("it works now"), a different case run in its place, a named step that never appears, or a vague result ("all tests pass", "no errors on my machine"). A step not run, or a check that fails, is honest evidence when the PR says so and explains why.

## Diff quality (harness category: unreviewable)

- **Where it lives.** Eval bundle: the candidate PR's "Diff" (the unified diff) and "Commits" list. Live mode: `git diff main...HEAD` and `git log main..HEAD`.
- **What good looks like.** The change is visible at a glance, and every hunk serves it. Debris includes debug prints, commented-out blocks, dead code, formatting-only churn, drive-by edits to unrelated files, dependency bumps the change does not need, and working notes such as plan or draft files. Commit messages like "gofmt" or "cleanup" next to a feature change are a common sign.

## Standards and comms (harness category: standards-wall)

- **Where it lives.** Eval bundle: the "Repo facts" block's pull-request line (the template's sections and checklist, contributing asks) and contribution-policy line (including any AI-use rule), plus the thread highlights for explicit maintainer direction, read against the candidate PR's title and description. Live mode: the repo's `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, any AI policy file, and the live thread, read against the draft PR text.
- **What good looks like.** Every section the template asks for has real content, or an honest "N/A" with a reason. Contributing asks that apply to this change are met, such as referencing the issue or adding a changelog entry. Any disclosure the policy or template asks for is present in plain words. The rubric's contributor-workflow input says whether an AI-use disclosure the repo asks for applies; when the repo states no AI-disclosure ask, none is needed. A template ignored outright, boilerplate in place of real sections, or a required disclosure missing is a standards wall.

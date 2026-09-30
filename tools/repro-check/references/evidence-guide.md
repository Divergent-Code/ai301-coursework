# Evidence guide: where proof lives in a reproduction package

## Environment

- **Where it lives.** Eval bundle: the repro report's environment line or section; compare it with the issue section's stated environment and the "bug reports: template asks for" line under Repo facts. Live mode: the draft repro comment, compared with the environment in the issue body and the repo's issue template (`.github/ISSUE_TEMPLATE/`).
- **What good looks like.** The report names the project version it actually ran. When that version or platform differs from the issue's, the report says so in words ("the issue was filed on 0.63.1; I tested 0.64.1"). Extra fields (OS, install method, build profile) matter only when the issue says the behavior changes with them, for example "debug builds panic, release builds wrap".

## Steps

- **Where it lives.** Eval bundle: the commands, inputs, and numbered steps in the repro report, plus the issue section's own steps if the report says it followed them. Live mode: the draft repro comment and the issue body.
- **What good looks like.** Someone who has never seen the author's machine can start from the stated starting state (a fresh repo, a named input file) and reach the trigger without guessing. Every input the command needs is shown or named. "Ran the issue's steps exactly" is enough when the issue's steps are themselves complete.

## Behavior shown

- **Where it lives.** Eval bundle: the output excerpts, logs, and error text pasted in the repro report, read against the symptom described in the issue section (its error message, panic text, or wrong output). Live mode: the pasted output in the draft repro comment, read against the issue body.
- **What good looks like.** The pasted artifact shows the same symptom the issue names: the same panic message, the same error, the same wrong result. A different error on similar input is a neighbouring failure, not this one; check the input actually matches the issue's (a typo in the input can produce a different error). For a cannot-reproduce, the artifact shows the issue's steps run as written and producing correct behavior.

## Honesty

- **Where it lives.** Eval bundle: every sentence in the claim comment and repro report that states what happened or what it means ("reproduced", "same here", "this confirms", "happens every time"), each read against the artifacts pasted beside it. Live mode: the same, in the draft comments.
- **What good looks like.** Each claim can be checked. The main result points at a pasted artifact ("the panic shown above"). A secondary observation without pasted output is fine when it names exactly what was changed and what was seen, so a stranger could re-run the report's own steps with that change and check it. A cannot-reproduce says what was run and what happened instead, which is a complete and honest result. Warning signs: claims about machines, runs, or people not shown ("on all my machines", "ran it ten times", "everyone I know"), and interpretations stronger than the artifact ("exactly the class of failure" beside a different error).

## Comms

- **Where it lives.** Eval bundle: the "contribution policy" line under Repo facts (including any AI-use disclosure requirement), read against the claim comment and repro report; the issue section and thread highlights, read against the claim comment. Live mode: `CONTRIBUTING.md`, the PR and issue templates, and any AI policy file in the repo, read against the draft comments.
- **What good looks like.** First check where each requirement applies: a rule aimed only at pull requests or code does not apply to issue comments, while a rule covering all contributions, or issues and comments by name, does. Then sort each requirement that applies into one of two kinds. A requirement to state something, such as disclosing AI assistance and its extent, is met only when the comments say it in plain words; since work in this course is AI-assisted, a comment that says nothing about AI does not meet a disclosure requirement. A requirement about how the text was produced, such as "written by a human", cannot be shown on the page and is met unless something contradicts it. If the policy is silent, nothing extra is required. The claim comment names this issue's specific behavior and the author's next concrete step; a "+1", "same here", or a claim that could be pasted onto any issue is not specific.

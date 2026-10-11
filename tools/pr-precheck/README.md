# pr-precheck

My Unit 4 skill. It grades a pull-request package (the plan, the diff, the PR's title and description, and its test evidence, read against the issue and the repo's own asks) and decides whether the PR is ready to open.

- `SKILL.md`: mine this unit, since staff shipped no working SKILL.md. It covers the modes (eval bundle or live draft), the scope and voice seams, how the other files are read, and the fixed JSON output from `CONTRACT.md`
- `rubric.md`: an Inputs line (`contributor-workflow`) and 5 required checks, each starting from a principle: diff-within-plan, description-matches-diff, tests-shown, diff-clean, meets-repo-asks
- `procedure.md`: the grading steps: read order, evidence gathering, check execution, verdict assembly
- `references/evidence-guide.md`: where each kind of evidence lives, and what good looks like
- `voice-guide.md`: my rules for writing upstream, carried over from Unit 2, plus three for pull requests (the title says what changed, promise exactly the diff, shortfalls as plain fact)
- `CONTRACT.md`, `scope.md`: staff-written; scope set to `codepath/pathreview-ai301-fa26-s1`

Final eval: 20/20 (PASS), recorded in `beat-1-sandbox/unit-4/eval-run.txt`. Live result on my own PR draft for #42: accept, 5/5 checks.

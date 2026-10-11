# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/127

Opened 2026-10-11 from `Divergent-Code:test/42-reviewpage-jest-axe` into `main`. It closes #42, and all five CI jobs passed. My own pr-precheck run on the final draft returned **accept**, with all 5 checks passing.

**Branch**

`test/42-reviewpage-jest-axe`

This follows Path Review's `docs/CONTRIBUTING.md` branch format, `<type>/<issue-number>-<short-description>`. I used `test` as the type because the change only adds tests; CONTRIBUTING's own example is `test/44-review-service-coverage`. The staff-written `scope.md` in my skill folder says `fix/<issue-number>-<slug>`. I left that file as shipped, because its header says not to edit it, and its own house rule says "The repo's stated asks are part of the bar, here exactly as in the wild." My live pr-precheck runs noticed the difference and didn't count it against the PR.

## Eval iterations

**Run history**

1. Smoke test on the first 3 packages: `agreement: 3/3 scored items`
2. Full run 1: `agreement: 14/20 scored items  (bar: 18/20: below the bar)`. Six clear-accept packages (pkg-05, 08, 11, 13, 16, 19) failed `tests-shown` and nothing else.
3. Canary 1, `--only` on those 6 packages plus the 4 not-tested packages, after loosening `tests-shown`: `agreement: 9/10 scored items`. Five of the six flipped to accept, pkg-11 stayed rejected, and all 4 not-tested packages were still rejected.
4. Full run 2: `agreement: 18/20 scored items  (bar: 18/20: PASS)`. pkg-08 now failed `meets-repo-asks`, because the grader read my `contributor-workflow` input as creating an AI-disclosure requirement in a repo that has none.
5. Canary 2, `--only pkg-08,pkg-01,pkg-20`, after rewording that check: `agreement: 3/3 scored items`
6. Full run 3, the committed `eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

**pkg-11** is a clear-accept package: the gold label is **accept**, and my rubric **rejected** it in every full run, failing only `tests-shown`. The run 3 row is `pkg-11  clear-accept    accept  reject   NO     failed: tests-shown`.

The plan's test plan says: "re-run the repro, expect no parse error and `foo ` to be ignored (parity with `git check-ignore`); `cargo test -p ignore`." The PR's evidence re-runs the repro, but gives each after-result as a comment on the command line, not as output:

```
$ rg hi                      # after: searches normally, no error
$ rg --files                 # after: 'foo ' not listed (ignored),
```

The grader's evidence line was: "After-state of `rg hi` and `rg --files` shown only as comments ('# after: searches normally, no error', '# after: ... not listed (ignored)'), with no pasted output, exit code, or count."

My rubric read it that way because `tests-shown` requires the plan's main reproduction to be re-run "with observable output after the change (pasted output, an exit code, or a test count)", and fails it when it is "claimed only in prose". A comment describing the result is a claim about the output, not the output. The other named step passed: "`cargo test -p ignore` passes (214 tests)" is a specific, checkable result. So only the main reproduction failed. My guess at why the gold label is accept: the comments name exactly what a stranger would see if they re-ran those two commands, which makes the claim easy to check. My rubric doesn't make that distinction, and I chose not to loosen it further (see Trade-offs).

**Check rationale**

```
| tests-shown | The PR's test evidence, read against the plan's main reproduction and each other step its test plan names (controls, extra failure modes, the repo's own checks or suite) | Principle: the evidence lets a stranger see the planned failure is gone. Pass if the plan's main reproduction is re-run as the plan names it, with observable output after the change (pasted output, an exit code, or a test count), and every other step the test plan names is reported as a specific result a stranger could check by re-running it (for example "`go test ./pkg/...` passes" or "the no-flag control is unchanged"). A named step that was not run passes only if the PR says so with a reason; a failing repo check passes only if it is shown and explained. Fail if the main reproduction is missing, replaced by a different case, or claimed only in prose; if a named step is silently missing; or if results are vague ("all tests pass", "works on my machine"). | required |
```

It reads this way because run 1 showed my first version was too strict. That version said: "Pass if the evidence shows an observable after-result for every failure mode or repro step the plan's test plan names, and the repo's own checks or suite were run with their outcome visible." The grader held every named step to that bar, so six clear-accept PRs failed over steps they had reported plainly. pkg-16, for example, said "`go test ./pkg/minikube/machine/... ./cmd/...` passes.", and the grader's evidence line was "a named required step with no pasted output, exit code, or test count."

So I split the check in two. The plan's main reproduction still needs observable output. Every other named step needs a specific result a stranger could check by re-running it, which is the same "checkable, not just pasted" idea as my repro-check rubric. I kept the fail cases for the real not-tested patterns: a main reproduction that is missing, replaced, or only claimed in prose, a silently skipped step, and vague results.

I rejected loosening the main reproduction as well, to accept a described after-result. That would probably have fixed pkg-11, but the main reproduction is the one place this check insists on seeing the result, and a described result is what "claimed only in prose" means.

**Trade-offs**

`tests-shown` gives up **pkg-11**, a clear-accept PR whose main reproduction gives its after-results only as comments. I accept that miss.

In exchange, it holds the line on not-tested PRs, and I checked that before spending a full run. Canary 1 ran `--only` on the six wrongly rejected clear-accepts plus the four not-tested packages. Five of the six flipped to accept, and all four not-tested packages stayed rejected: `agreement: 9/10 scored items`. Run 3 confirmed it with `not-tested 4/4`.

The check also earns its place on its own. In run 3 it failed six packages. Five were correct rejections (pkg-04, 07, 10, 14, 17), and pkg-11 was the one wrong one. For pkg-07 it was the only failing check: the evidence "runs only 'a single-file case' (the plan's control), not the plan's named main reproduction". So without `tests-shown`, pkg-07 would have been accepted.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

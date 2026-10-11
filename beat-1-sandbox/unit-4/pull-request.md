# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/127

Opened 2026-10-11 from `Divergent-Code:test/42-reviewpage-jest-axe` into `main`. It closes #42, and all five CI jobs passed. My own pr-precheck run on the final draft returned **accept**, with all 5 checks passing. I re-ran it with the revised version of the skill that is uploaded here, and it returned **accept** again, with all 5 checks passing.

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
6. Full run 3: `agreement: 19/20 scored items  (bar: 18/20: PASS)`
7. After my Unit 4 grader feedback, I made one file the source of truth for each rule, split `tests-shown` and `meets-repo-asks` into numbered clauses, and proofread `SKILL.md`. Smoke test on the first 3 packages: `agreement: 3/3 scored items`
8. Canary, `--only pkg-04,pkg-07,pkg-10,pkg-14,pkg-01,pkg-20,pkg-08,pkg-11`: every not-tested and standards-wall package, plus the two packages earlier fixes were about. `agreement: 8/8 scored items`. pkg-11 was accepted for the first time.
9. Full run 4, the committed `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. The only row that changed from run 3 is pkg-11, reject to accept.

**Package analysis**

**pkg-11** is a clear-accept package: the gold label is **accept**. My rubric rejected it in full runs 1–3, on `tests-shown` alone, and accepted it in full run 4. The run 3 row was `pkg-11  clear-accept    accept  reject   NO     failed: tests-shown`. The run 4 row is `pkg-11  clear-accept    accept  accept   yes`.

The plan's test plan says: "re-run the repro, expect no parse error and `foo ` to be ignored (parity with `git check-ignore`); `cargo test -p ignore`." The PR re-runs the repro, but gives each after-result as a comment on the command line:

```
$ rg hi                      # after: searches normally, no error
$ rg --files                 # after: 'foo ' not listed (ignored),
```

In run 3, the grader read those comments as claims: "After-state of `rg hi` and `rg --files` shown only as comments ('# after: searches normally, no error', '# after: ... not listed (ignored)'), with no pasted output, exit code, or count." That is clause F1, "claimed only in prose". In run 4, it read the same lines as output: "P1: plan repro re-run with before/after output and rg --files parity on 'foo '; P2: 'cargo test -p ignore passes (214 tests)'."

The package didn't change, and neither did the meaning of P1: I only split the old sentence into numbered clauses. So I can't credit the rewrite with the flip. In the same run 4, the grader failed pkg-20 under F1 for the same kind of evidence: "repro before/after appear only as '# before/# after' comment annotations". pkg-20 is still a correct reject, because it also fails `meets-repo-asks` for a missing AI disclosure. But the two reads disagree. My rubric doesn't say whether a comment stating the after-result counts as observable output, so pkg-11 sits right on that line, and the grader can land on either side of it.

**Check rationale**

```
| tests-shown | The PR's test evidence, read against the plan's main reproduction and each other step its test plan names (controls, extra failure modes, the repo's own checks or suite) | Principle: the evidence lets a stranger see the planned failure is gone.<br>**Pass** only if both hold:<br>**P1.** The plan's main reproduction is re-run as the plan names it, with observable output after the change: pasted output, an exit code, or a test count.<br>**P2.** Every other step the test plan names is reported as a specific result a stranger could check by re-running it, for example "`go test ./pkg/...` passes" or "the no-flag control is unchanged".<br>**Exceptions to P2:**<br>**E1.** A named step that was not run still passes if the PR says so and gives a reason.<br>**E2.** A failing repo check still passes if the PR shows its output and explains it.<br>**Fail** if any of these is true:<br>**F1.** The main reproduction is missing, replaced by a different case, or claimed only in prose.<br>**F2.** A named step is silently missing.<br>**F3.** A result is vague, such as "all tests pass" or "works on my machine". | required |
```

It reads this way after two revisions.

**After run 1, I split the check.** My first version said: "Pass if the evidence shows an observable after-result for every failure mode or repro step the plan's test plan names, and the repo's own checks or suite were run with their outcome visible." The grader held every named step to that bar, so six clear-accept PRs failed over steps they had reported plainly. pkg-16, for example, said "`go test ./pkg/minikube/machine/... ./cmd/...` passes.", and the grader's evidence line was "a named required step with no pasted output, exit code, or test count." So now only the main reproduction needs observable output (P1). Every other named step needs a specific result a stranger could re-run (P2), the same "checkable, not just pasted" idea as my repro-check rubric.

**After my Unit 4 grader feedback, I split the sentence.** The rule had become one roughly 90-word sentence, and it was restated in the evidence guide and the rubric's notes. I broke it into numbered clauses (P1–P2, E1–E2, F1–F3) and made `rubric.md` the only file that states it. The evidence guide, the notes and the procedure now point to it by clause. The procedure also asks the grader to start each evidence line with the deciding clause, so a failure shows which clause did it. In run 4, pkg-10's line begins "F1: the plan's 350 ms delayed-connect repro was not re-run…".

**What I rejected:** loosening P1 to accept a described after-result. That would probably make pkg-11 a steady accept. But P1 is the one place this check insists on seeing the result, and a described result is what F1 calls "claimed only in prose".

**Trade-offs**

`tests-shown` gives up certainty on **pkg-11**. Run 4 accepted it, but I accept that a rerun could reject it again. In the same run, the grader read pkg-11's `# after:` comments as output and pkg-20's `# before/# after` annotations as prose, and nothing in P1 settles which reading is right. I chose not to add a clause for comment-stated results. That would be a real rule change, needing its own canary and full run, and it could just as easily loosen the read on pkg-20's test evidence.

Here's how I know the rewrite moved nothing else. Before the full run, a canary ran `--only` on every not-tested and standards-wall package plus pkg-08 and pkg-11, and came back `agreement: 8/8 scored items`. Then full run 4 changed exactly one row from run 3: pkg-11, reject to accept. The other 19 verdicts are unchanged, and the categories again read `not-tested 4/4` and `standards-wall 2/2`.

The check still earns its place on its own. In run 4 it failed seven packages, all correct rejections (pkg-04, 06, 07, 10, 14, 17, 20). For pkg-07 and pkg-10, it was the only failing check. For pkg-07, for example: "F1: the evidence runs only the single-file control ('golangci-lint run --fix ./onefile/'); the plan's two-file repro and 'go build ./...' are absent". Without `tests-shown`, both would have been accepted.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

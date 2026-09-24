# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/42

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
## Grading: Issue #42 — `codepath/pathreview-ai301-fa26-s1`

**Issue:** "Add accessibility tests for the review page using `jest-axe`" — opened by Aburke225, labels `frontend`, `tests`, `tier-2`, no comments, no assignees.

**Scope check:** confirmed inside `codepath/pathreview-ai301-fa26-s1`, the only in-scope source per `scope.md`.

| Check | Grade | Evidence |
|---|---|---|
| maintainer-active | pass | Last 5 default-branch commits all authored by non-bot `Aburke225`, latest dated 2026-09-16 (8 days before capture date 2026-09-24) |
| maintainer-responsive | unclear | Sampled issues (e.g. #72, #73) were opened by `Aburke225`, the repo owner/maintainer — no sampled issue meets "not opened by a maintainer," so the rubric's fallback applies. Preferred check only; no effect on verdict |
| repo-in-use | pass | `isArchived: false`; no releases exist, so fallback applies: last push `2026-09-16` (8 days before capture date) is within 30 days |
| scope-bounded | pass | Single-file, single-PR deliverable ("Add jest-axe tests" to `frontend/src/pages/__tests__/ReviewPage.test.tsx"); no umbrella language, no unresolved design debate (empty thread), no maintainer note about core internals, not a support question, no `needs-design`/`blocked`/`wontfix`/`stale` label |
| not-claimed | pass | `assignees: []`, `comments: []` on the issue itself; only open PR in the repo (#74) targets a different issue (#60) — no PR references #42 |
| ai-policy-ok | pass | `docs/CONTRIBUTING.md` has no AI-use policy — silence passes per rubric |
| no-failed-history | pass | Issue opened 2026-09-10, 14 days old — nowhere near the 1-year threshold |

**Verdict driver:** every required check passed.

**Fit note:** this one lands on the "UI" side of your stated interest (JS/TS testing in the React frontend, adding `jest-axe` coverage) — a reasonable match even without competing candidates to rank it against.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/42",
  "checks": [
    {"name": "maintainer-active", "grade": "pass", "evidence": "Last 5 default-branch commits authored by non-bot Aburke225, latest 2026-09-16"},
    {"name": "maintainer-responsive", "grade": "unclear", "evidence": "Sampled issues (#72, #73) were opened by maintainer Aburke225; no sample meets 'not opened by a maintainer' precondition"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "Not archived; no releases, last push 2026-09-16 is within 30 days of capture date"},
    {"name": "scope-bounded", "grade": "pass", "evidence": "Single bounded ask (add jest-axe tests to one file); no umbrella/debate/core-internals/support-question/blocking label"},
    {"name": "not-claimed", "grade": "pass", "evidence": "assignees: [], comments: [], no linked or mentioned PR references #42"},
    {"name": "ai-policy-ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md is silent on AI use — silence passes per rubric"},
    {"name": "no-failed-history", "grade": "pass", "evidence": "Issue opened 2026-09-10, 14 days old, far under the 1-year threshold"}
  ],
  "verdict": "accept"
}
```
````

*Note: this live run added one instruction, "Refer to the user as 'the student', never by name. Do not guess anyone's pronouns", to keep personal details out of a public file. The rubric, scope and skill files were unchanged. An earlier run on #56, #68 and #42 without that instruction also accepted #42, and is where the ranking in the Selection rationale comes from.*

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke test (`--limit 3`): no score. The harness crashed before grading with `UnicodeEncodeError: 'charmap' codec can't encode characters` (Windows default encoding); re-run with `PYTHONUTF8=1`.
2. Smoke test (`--limit 3`): `agreement: 2/3 scored items`
3. Full run 1: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`
4. Re-check (`--only issue-01,issue-19,issue-20`): `agreement: 3/3 scored items`
5. Full run 2: `agreement: 18/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)`
6. Re-check (`--only issue-09,issue-12,issue-15`): `agreement: 3/3 scored items`
7. Full run 3 (the committed `eval-run.txt`): `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

My rubric accepted issue-20 in full run 1 (`issue-20  reject  accept   NO     graded accept`), and the gold label is reject. In full run 3 my rubric rejects it, matching the gold label.

Run 1 accepted it because every required check passed. The repo was active and not archived, the issue was unclaimed, and the contribution policy was silent. None of my checks looked at who opened the issue. The bundle shows `opened by cursor[bot] (NONE) on 2026-08-02, state open, labels: none`, with 0 comments. A bot opened it and no maintainer has accepted it, so it's a feature request that no person on the project asked for or approved.

I added scope rule (6): fail if the issue was opened by a bot account, no maintainer has commented, and it has no labels. I didn't use the broader rule "fail if no maintainer has accepted it", because issue-01 was opened by a contributor, has 0 comments, and its gold label is accept. The broader rule would have fixed issue-20 and broken issue-01. In run 3 the grader cites the new rule: "Opened by cursor[bot], 0 comments (no maintainer engagement), no labels — matches exclusion (6)".

**Check rationale**

Quoted from `tools/issue-select/rubric.md`:

`| no-failed-history | Issue open date vs the capture date; linked PRs and PRs mentioned in the comment thread, with their state | Fail if the issue was opened more than 1 year before the capture date AND has at least 2 closed, unmerged PRs (several abandoned attempts). In the repo-facts "linked PRs" field, a PR marked "(closed)" is closed without merging; "(merged)" is merged. Otherwise pass. | required |`

I first added this check as preferred, with 1 year and 1 PR, because an old issue with failed attempts is a warning sign but not every old issue is bad. In full run 2 my rubric accepted issue-15 (gold: reject). It has been open since 2021 with 2 closed, unmerged PRs, and a preferred check can't reject anything. So I promoted it to required. With a threshold of 1 PR it would also have rejected issue-09 (gold: accept), which has only 1 closed PR, so I raised the threshold to 2, matching the evidence guide's wording "several abandoned attempts". I added the sentence about "(closed)" and "(merged)" so the grader doesn't have to guess what a closed PR means.

**Trade-offs**

The threshold of 2 gives up old issues with exactly one abandoned PR. An issue open for years with a single failed attempt will pass, even if that attempt failed because the work is harder than it looks. I accept that miss, because one failed PR can happen for reasons unrelated to the issue.

Before spending a full run I checked the effect: in run 2's results this check failed issue-09 (gold: accept), so I re-ran `--only issue-09,issue-12,issue-15` with the threshold at 2, and issue-09 came back `accept` while issue-15 stayed `reject`.

The threshold is also partly tuned to this eval set. It happens to split issue-03, 09, 15 and 18 exactly right. The evidence guide's "several abandoned attempts" is my reason for 2, but a different set of issues might need a different number.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time:** #42 fits because #42 would look good for my portfolio that is accessibility focused. It's also frontend work, which I want to get better at. It's one test file, so it fits the time before Unit 2 is due on September 28.
2. **What the verdict got right, and what I weighed that it couldn't:** my skill correctly found the repo active, the issue unclaimed, the scope bounded to one file, and no AI policy. My fit profile didn't mention accessibility, so the skill ranked #42 third behind #56 and #68. I chose it anyway because accessibility matters for my portfolio, and the rubric had no way to know that.
3. **Anticipated difficulty in claiming it:** #42 adds tests rather than fixing a bug, so "reproducing" it means showing the review page has no accessibility checks yet, not showing a crash. I'll also need to set up the frontend (Node/npm) as well as the Python side.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

# Toolkit Portfolio

**Student:** Onika Dye-Flynn

Written with the pick-list method my instructor approved on 2026-11-09:
I chose every fragment, and Claude put my picks into sentences.

## Featured Tools

I chose these three because they follow one issue from start to finish.
issue-scout graded galaxy #15551, plan-check checked my plan for it, and
pr-precheck checked my pull request. Each one also caught something real
on that contribution: the scout accepted the issue, plan-check rejected a
plan that overstated what I knew, and pr-precheck blocked my PR when
evidence was missing.

**issue-scout**

**What it is, and the judgment hole I wrote:** It searches GitHub for
first-issue candidates, ranks them, and grades the top 5 against my
rubric. I wrote three parts of it. My search strategy sets its queries
and its ranking rules: issues with accessibility labels come first, no
more than 2 come from any one repo, and a dated skip list. My fit
profile says what I'm looking for: accessibility first, then usability
and UX, in Python, TypeScript and React. My rubric is the 7 checks it
grades each candidate against, including a 10-star minimum in
`repo-in-use`.

**How it evolved, dated:**

- **2026-10-14:** My quoted search query found 0 results and still
  exited with code 0. I unquoted the words, and it found 100.
- **2026-10-14:** 3 of the scout's 4 accepts were repos 3 to 16 days
  old, with 0 to 3 stars. I added a 10-star minimum to `repo-in-use`,
  and the next scan rejected 3 of them.
- **2026-10-16:** Issues that someone had already claimed kept coming
  back. Pluto #3596 and #3274 were each rejected twice. I added a skip
  list that drops each one for 30 days.
- **2026-10-23:** I replaced my `scope-bounded` check with the course's
  `scope-feasible` check. It now rejects a codebase-wide checklist issue
  that the old check passed, and my rubric scored 19/20 on the eval.

**Evidence:** The scout accepted galaxy #15551, the issue I contributed
to, on 2026-10-16. On 2026-10-23 it accepted it again with the new
rubric. Part of its output:

```text
### 2. galaxy #15551: ACCEPT
- **Fit:** it is a usability and error-message fix. It has the `area/UI-UX`, `paper-cut` and `help wanted` labels.
- **Difficulty note: moderate.**
  - You would have to find where the error is raised, probably across client and server.
```

The start of the rubric row I replaced, before and after:

```text
Before (2026-10-14):
| scope-bounded | Issue body, comment thread (with each commenter's author_association), and the issue's labels | Fail if any of these apply: (1) it is an umbrella or tracking issue …

After (2026-10-23):
| scope-feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it …
```

**pr-precheck**

**What it is, and the judgment hole I wrote:** It grades my pull request
(the diff, title and description) against my plan and the issue, then
says whether it's ready to submit. I wrote its rubric: 5 checks,
`diff-within-plan`, `description-matches-diff`, `tests-shown`,
`diff-clean` and `meets-repo-asks`. My `tests-shown` check has numbered
clauses: the main reproduction has to be re-run with visible output, a
skipped step passes only with a reason given, a step that's silently
missing fails, and a vague result fails.

**How it evolved, dated:**

- **2026-10-11 (commit `bc19d0c`):** My first eval run scored 14/20. Six
  good PRs failed `tests-shown` over steps they had reported plainly. I
  split the check so only the main reproduction needs pasted output, and
  run 2 scored 18/20.
- **2026-10-11 (commit `bc19d0c`):** Run 2 failed a good PR on
  `meets-repo-asks`, because the grader read in an AI-disclosure rule the
  repo doesn't have. I reworded the check, and run 3 scored 19/20.
- **2026-10-11 (commit `21974f8`):** After my grader's feedback, I split
  the rule. It had grown into one sentence of about 90 words, repeated in
  several files. Now it's numbered clauses kept in one file, and run 4
  scored 20/20.

**Evidence:** My check caught my own galaxy PR. On 2026-11-05 at 19:18,
the ship-gate blocked it because `tests-shown` failed:

```text
tests-shown [fail]: F2: plan step 4 names `make diff-format`, but neither the description nor the evidence file says it was not run or why (the reason is only in plan.md Deviations); F3: flake8 is reported only as 'flake8 done'.
```

I added the reason and each check's exit code to my evidence. At 19:19
the same check passed:

```text
tests-shown [pass]: P1: API listing re-run after fix shows `{"err_msg":"No Dropbox access token is set...` and `HTTP status: 403`; P2 steps all have specific results; E1: `make diff-format` not run, with reason given.
```

The F2 and E1 clauses that decided it came from my 2026-10-11 rewrite.

**plan-check**

**What it is, and the judgment hole I wrote:** It grades my plan and
plan comment against the issue and my reproduction, then says whether
they're ready to post and build from. I wrote its rubric: 7 checks,
`cause-grounded`, `scope-bounded`, `executable`, `test-flips-repro`,
`certainty-earned`, `thread-aware` and `follows-conventions`. My
`certainty-earned` check works from the principle that "the plan is
only as certain as its evidence". It fails a plan that states something
as settled when the evidence doesn't show it.

**How it evolved, dated:** It barely changed.

- **2026-10-06 (commit `1993c9f`):** My rubric's one full eval run
  scored 19/20. The one miss was a good plan it rejected over one
  inferred phrase. I kept the check strict rather than loosen it for
  that, and the rubric is still the same file today.
- **2026-11-04:** It graded a real plan for the first time, my galaxy
  plan.

**Evidence:** On 2026-11-04 the first run rejected my galaxy plan on
`certainty-earned`:

```text
## Verdict: reject (hold)
…
- **"An empty token fails inside the Dropbox library with `BadInputException`".** Your repro shows only the final 403 message. It doesn't show the token's value or the exception type.
```

So I called `_open_fs` directly, with the Dropbox preference empty:

```text
access_token repr: ''
…
caused by: dropbox.dropbox_client.BadInputException
```

With that output in the plan, the second run accepted it: "All seven
required checks pass."

## Inventory

- **issue-select:** my Unit 1 tool, which grades whether an issue is a
  good first issue. In Unit 5, issue-scout was installed on top of it, so
  its rubric became the scout's, and 6 of its 7 checks still have the
  same names there.
- **repro-check:** grades my claim comment and repro report before I post
  them. I used it for real twice: it accepted my galaxy claim on
  2026-10-29 and my repro report on 2026-11-04. On the claim, it flagged
  one line as close to my rule "promise the look, not the fix".
- **voice guard:** holds my comment or PR text against my voice guide
  before `gh` posts it. It passed my galaxy claim, my findings comment and
  my PR. It stopped the claim once because it couldn't read the body
  file, and the retry passed.
- **galaxy memory file:** `CLAUDE.local.md` in my galaxy clone, with
  galaxy's conventions, the Dropbox code and its commands. It's 59 lines,
  and every command in it ran on 2026-11-04.
- **ship-gate:** stops `gh pr create` until my 4 manifest checks and my
  pr-precheck pass. It blocked my PR once, on 2026-11-05 at 19:18, passed
  a minute later, and the PR opened through it at 19:24.
- **PR monitor:** reads my PR's state and drafts my next move from my
  playbook. On 2026-11-05 it drafted WAIT, and the first nudge date is
  around 2026-11-13 if the PR is still quiet.

## The Arc

**Beat 1**
Date: 2026-09-24
Friction: My issue-select eval's first full run scored 17/20. Its results showed it accepted issue-20, which a bot opened and no maintainer had commented on or labeled.
Change: I added a rule that fails an issue opened by a bot when no maintainer has commented and it has no labels.
Result: By run 3 it rejected issue-20, citing the new rule, and scored 20/20.

**Beat 2**
Date: 2026-09-30
Friction: My repro-check eval's first full run passed pkg-20, whose repo requires AI disclosure. The grader read the drafts' silence as "no AI was used".
Change: I added the principle that work in this course is AI-assisted, so a missing disclosure fails.
Result: Full run 2 scored 20/20, and pkg-20 was the only row that changed.

**Beat 3**
Date: 2026-10-16
Friction: Searching issue titles for HCI words gave noisy picks, like "usability predicates" in a finance library. I saw it when I simulated the ranking.
Change: I graded issues carrying HCI tags on the course's Issue Selection site instead.
Result: The scout accepted galaxy #15551, the issue I went on to contribute to.

**Beat 4**
Date: 2026-11-04
Friction: plan-check rejected my galaxy plan on `certainty-earned`. The plan stated `BadInputException` as fact, and my repro showed only the final 403 message.
Change: I called `_open_fs` directly, which showed the token was `''` and the error started as `BadInputException`, and put that output in the plan.
Result: The second run accepted the plan. My Assignment 5 grader later named that step as part of my strongest habit.

**Beat 5**
Date: 2026-11-05
Friction: The ship-gate blocked my PR at 19:18. My pr-precheck said my evidence gave no reason for skipping `make diff-format`, and reported flake8 only as "flake8 done".
Change: I added the reason and each check's exit code to my evidence.
Result: The gate passed at 19:19, and PR #24755 opened through it at 19:24.

**Beat 6**
Date: 2026-11-06
Friction: GitHub notifications showed two CI jobs failing on my new test. CI doesn't install `fs.dropboxfs`, which plan-check had warned about on 2026-11-04.
Change: I added `pytest.importorskip("fs.dropboxfs")`, the way galaxy's Azure and S3 tests do.
Result: Locally, the test passes with the package and skips without it. CI hasn't re-run yet, because it's waiting for a maintainer to approve it.

## Limitations and What's Next

My toolkit still has two gaps. Nothing checks the CI environment: every
check ran in my local venv, which had `fs.dropboxfs`, and CI didn't, so
my new test failed there on 2026-11-06. Nothing looks at the repo's
existing code before I write mine, either. The one thing my Assignment 5
grader said to tighten was to copy the two or three closest similar
files first, and none of my tools prompts that.

Next, I'll add a CI check to my ship-gate manifest: run the new test
with optional packages blocked, the way I did by hand on 2026-11-06.

**Tool limits.**

issue-scout: It takes GitHub's license label at face value. It reported galaxy's license as "Other" three times, because GitHub can't classify it, while galaxy's LICENSE.txt says MIT.
pr-precheck: It only checks what my evidence says. It passed my PR on 2026-11-05, and CI still failed on 2026-11-06, because nothing in my evidence covered CI.
plan-check: It warns but can't make me act. It flagged `fs.dropboxfs` on 2026-11-04 as a side note outside its verdict, and I didn't follow it up.

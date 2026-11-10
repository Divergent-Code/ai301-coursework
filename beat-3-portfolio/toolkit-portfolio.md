# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.

**Student:** [Student name]

## Featured Tools

Three or four tools, presented in depth. Open with a few sentences
on why you chose these.

Full credit asks the featured set to show real change: **at least two** of your featured
entries should show a tool that visibly moved through use. An honest entry for a tool that
barely changed — saying so, dating what did happen, and giving the reason — earns the solid
middle tier and never fails you.

Each tool's honest limit is not written here. It goes in the tool-limits list at the
bottom of this page, one line per featured tool, so that the limits sit together and can be
read as a set.

I chose these three because they follow one issue from start to finish.
issue-scout graded galaxy #15551, plan-check checked my plan for it, and
pr-precheck checked my pull request. Each one also caught something real
on that contribution: the scout accepted the issue, plan-check rejected a
plan that overstated what I knew, and pr-precheck blocked my PR when
evidence was missing.

Copy the entry shape below once per featured tool and fill it there.

**[Tool name]**

**What it is, and the judgment hole I wrote:** [what the tool does,
and the part of it that is your judgment: the rubric, strategy,
guide, manifest, or playbook you wrote.]

**How it evolved, dated:** [the changes that made it yours, each
with its date and the friction that drove it; your weekly Toolkit
entries are the source.]

**Evidence:** [show, don't claim: an output excerpt, a before/after
pair, or a dated commit reference, inline here. Draw it from your real
term work where you can - a catch this tool actually made, a
before/after from your own repo or PR - which is graded separately from
whether an artifact is present at all. A short, telling excerpt beats a
pasted transcript.]

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

One line each for the rest of the toolkit: what it is and where it
stands (one bullet per tool; a compact sentence or two is fine).

- [Tool name: one line.]

## The Arc

The story of the toolkit in order: friction, change, result, dated.
Your weekly Toolkit entries are the source; read your own record and
tell it oldest first.

Write one block per beat, in the shape below, oldest first. Keep all four labels on every
beat, each on its own line. The `Result:` line is what the beat was for: it says what
happened *after* the change, which is not the same sentence as the change itself. If a beat
never reached a result, write `Result: none` — an honest beat that did not land still counts
fully as a beat.

Full credit asks for **at least two** beats whose `Result:` line names something that
actually happened afterwards. A beat about something that did not work counts fully as a
beat, and when the work happened never moves this score.

**Beat 1**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

**Beat 2**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

Copy the beat shape above for as many beats as your term had.

## Limitations and What's Next

What the toolkit as a whole still does not do, and what you would
build or change next. Write this section last; it will be better
after the rest.

[The honest limits, and what comes next, in your own sentences.]

**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.

issue-scout: [where this tool still stops, in plain words, or the word none]
[tool name]: [where this tool still stops, in plain words, or the word none]

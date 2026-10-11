---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this pull request ready to submit? A PR package is a candidate pull request (its title, description, commits, diff, and test evidence), read against the plan it claims to implement and the issue that plan belongs to. Grade exactly one package per run. Do not answer any other question, such as whether the code is good style or whether the issue is worth fixing. Do not answer from gut feel: execute the components in this directory.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Gather these inputs:

- **The plan:** `plan.md` in the student's working copy, including its Deviations section. A house-chain student reads the house plan instead.
- **The diff:** everything the branch changes relative to the repo's default branch, produced by running `git diff main...HEAD` (three dots) from the working copy. Use the commit list from `git log main..HEAD` alongside it.
- **The draft PR text:** the file the student names (for example `pr_draft.md`). Its first line is the title; the rest is the description.
- **The test evidence:** the file the student names (for example `test_evidence.md`), holding the captured commands and output. A house-chain student reads the house repro pack for the reproduction steps.
- **The issue:** the issue URL the student gives. Read its thread, the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`), and its `CONTRIBUTING.md` and any AI policy from the real repo, via `gh` or the web.

Read the drafts the way a maintainer will read the opened PR: the package is what the diff, the draft text, and the evidence file contain, not other files in the working copy.

**Eval mode** grades a package bundle. The bundle is the whole world: take every fact from the bundle text, fetch nothing, and read no other file. Always grade a complete package: run every check and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Confirm that the PR targets the repo its `Repo:` line names; if it targets any other repo, refuse to grade and say why. If the `Repo:` line still shows a bracketed placeholder, stop without grading and tell the student to get their cohort's scope file from the instructor. Never guess a scope. Apply the house rules `scope.md` states when you read the evidence they touch. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`. Hold the outgoing PR text, the title and the description, against its rules. For each rule the draft breaks, quote the rule and the line that breaks it in the summary. The voice guide never changes the verdict on its own: only rubric checks decide it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this directory and use each for its job:

- `rubric.md` defines the checks, its Inputs line, and the verdict rule. It decides what passes.
- `references/evidence-guide.md` maps where each evidence family lives in a package, and in live mode, and what good looks like.
- `procedure.md` is the operating procedure. Execute it as written, in its order.

When the procedure is silent on a step, say so in the summary as a procedure gap; do not improvise around it. If `rubric.md` has no checks, or `procedure.md` has no steps, refuse to grade and say which file is empty: this tool cannot grade without a rubric and a procedure.

## Verdict and output

Give one of two verdicts: `accept` (ready to submit) or `reject` (hold). There is no third verdict; put reservations in a check's evidence line instead. You may give a short readable summary first, with one line per check and, in live mode, any voice-guide notes. End the reply with this fenced JSON block, valid and filled in, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff, the evidence, and the description against the plan, the issue, and the repo's stated asks. A terse complete PR can be ready; a confident, polished one can be hiding drift.
- **The rubric decides.** If a check passes by its stated condition but feels wrong, it passes. Note the tension in the summary; the fix belongs in the rubric.
- **The procedure decides how.** Follow `procedure.md` as written and report its gaps instead of inventing steps.
- **Unclear counts as fail** unless the rubric's verdict rule says otherwise: a claim you cannot verify from the package is not ready to submit.

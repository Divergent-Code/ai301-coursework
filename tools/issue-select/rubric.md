# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo facts: the last 5 default-branch commits (dates and authors) | Pass if at least one default-branch commit dated within 30 days of the capture date was authored by a non-bot (an author name not ending in `[bot]`), or is a bot merging a human's pull request. | required |
| maintainer-responsive | Repo facts: the maintainer first-response sample (days to first owner/member/collaborator comment, who opened each sampled issue, and its open date) | The sample marks maintainer-opened issues with "by a maintainer"; an entry without that marker was opened by an outsider. Count only sampled issues that were NOT opened by a maintainer AND were opened at least 14 days before the capture date. Pass if at least half of those got a maintainer reply within 14 days ("no maintainer comment in thread" counts as no reply). If no sampled issue meets both conditions, grade unclear. | preferred |
| repo-in-use | Repo facts: latest release (and its date), last push to any branch, and the archived flag on the repo line | Pass if archived is no AND either the latest release is dated within 180 days of the capture date, or (only when the repo has no releases) the last push to any branch is within 30 days of the capture date. | required |
| scope-bounded | Issue body, comment thread (with each commenter's author_association), and the issue's labels | Fail if any of these apply: (1) it is an umbrella or tracking issue: the issue calls itself a tracker, umbrella, meta, or epic issue, or it links out to separate sub-issues. A list of steps that together deliver one change, in one pull request, is not an umbrella; (2) the thread shows the design is still being debated and no maintainer comment has settled it; (3) a maintainer says the fix touches core internals; (4) it is a usage or support question, not a request for a code change; (5) it carries a `needs-design`, `blocked`, `wontfix`, or `stale` label (a label marking it recovered from stale, like `stale::recovered`, does not count); (6) it was opened by a bot account (an author name ending in `[bot]`) AND no maintainer (owner, member, or collaborator) has commented on it and it has no labels. Otherwise pass. A short or unpolished write-up is not a fail on its own. Items the issue marks as optional ("additional suggestions", "nice to have", "lower priority") are not part of the scope being graded. | required |
| not-claimed | Repo facts: "this issue: assignees" and "linked PRs" with each PR's state; plus the comment thread, including PRs mentioned there but not formally linked | Fail if any of these apply: (1) anyone is assigned; (2) there is an open or merged PR for this issue, whether linked or mentioned in the comments; (3) a comment claiming the work ("I'll take this", "can I work on this", "working on this") is dated within 30 days of the capture date. Otherwise pass. A closed, unmerged PR does not fail this check. When the linked-PR field and the thread disagree, believe the thread. | required |
| ai-policy-ok | Repo facts: the "contribution policy" line | Fail if the policy bans all AI-assisted contributions. Pass if it states conditions (disclosure, personal understanding, testing, human review), or bans AI-generated work but explicitly states that assistive AI use is allowed, or states no policy. A policy that rejects "AI-generated" contributions without that explicit allowance for assistive use is a ban, and fails. | required |
| no-failed-history | Issue open date vs the capture date; linked PRs and PRs mentioned in the comment thread, with their state | Fail if the issue was opened more than 1 year before the capture date AND has at least 2 closed, unmerged PRs (several abandoned attempts). In the repo-facts "linked PRs" field, a PR marked "(closed)" is closed without merging; "(merged)" is merged. Otherwise pass. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Unclear counts as fail. Preferred checks never change the verdict; they only rank accepted issues.

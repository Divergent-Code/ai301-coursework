# Contribution: Remote files: Dropbox could use nicer error message when unconfigured

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Onika Dye-Flynn
**GitHub Username:** Divergent-Code
**Issue:** https://github.com/galaxyproject/galaxy/issues/15551
**Pull Request:** https://github.com/galaxyproject/galaxy/pull/24755

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: installed the scout, ran two scans, and added rubric rules (2026-10-14)**

This week I installed the scout, updated my profile, chose a query and ranking rules, ran two scans, and graded my picks. Two things broke: my quoted query returned zero results with exit code 0, and 3 of the scout's 4 accepts were tiny new repos with 0-3 stars. So I unquoted the words in my query, which took it from 0 to 100 results, and added a 10-star floor and claim rules to my rubric. In my second scan the star floor rejected 3 of the tiny repos, and my rubric still scored 20/20 on the Unit 1 eval.

**Unit 6: installed the voice guard and wrote galaxy's memory file (2026-11-04)**

I installed the voice guard and wrote a 54-line memory file for galaxy with its conventions, the subsystem my issue touches, and its commands. A curly quote crashed the guard, and a crash lets the post through, so I fixed it with `PYTHONUTF8=1`. The guard also stopped my claim once, because a chained `; echo` was read as the filename, and the retry passed. The memory file's test and lint commands aren't confirmed yet, so they're marked not run.

**Unit 7: installed the ship-gate and opened my PR through it (2026-11-05)**

I installed the ship-gate and wrote a manifest of four galaxy checks: the file-source tests, ruff, black and isort. All four passed, but my pr-precheck rejected the evidence on the first run, so I fixed it and the second run passed. The ship-gate runs checks through `cmd.exe` on Windows, so I ran everything inside WSL.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

This was one of my site picks, and my scout accepted it, with a moderate
difficulty note. I scanned GitHub 7 times and graded my picks. One scan
surfaced NVDA #21022, so I first chose that issue, and my instructor
said yes. Then someone opened a PR for it, so I came back to galaxy. I
treated the scout's verdict as evidence, and the final call was mine.
This issue fits my HCI focus, because clear error messages are what I
care about. The error shows a raw Python object that tells a user
nothing. I hope to learn how to trace an error from the screen back to
the server.

The scout fell short in two ways. It said galaxy's license was Other,
but the file says MIT, so I now read the file, not the field. And a
verdict is a snapshot: it accepted NVDA on Oct 20, and it was taken by
Oct 26. I changed my search to repo-first queries with a dated skip
list, and I patched the scope check to scope-feasible, which scored 19
of 20 on the eval.

Claim comment: https://github.com/galaxyproject/galaxy/issues/15551#issuecomment-6644678608

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I scanned GitHub 7
  times, graded my picks, and treated its verdict as evidence; the final
  call was mine.
- What it produced: 7 scans, and a verdict of accept on this issue with
  a moderate difficulty note.
- Difficulties: it said galaxy's license was Other when the file says
  MIT, and a verdict is a snapshot: it accepted NVDA on Oct 20, and it
  was taken by Oct 26.
- What changed: repo-first queries, a dated skip list, and a scope check
  patched to scope-feasible (eval 19 of 20).

---

## Understanding the Issue

**Problem Description**

Listing Dropbox with no token used to show a raw Python object, and
that's now gone. What's left is the wording.

**Expected Behavior**

It should say no token is set and point to preferences, without
"Permission Denied" or OAuth2 wording.

**Current Behavior**

With no token set, `dev` (`7e16f44`) and `release_25.1` (`07d653f`) both
return HTTP 403: "Permission Denied. Reason: OAuth2 access token or
refresh token or app key/secret must be set. Please check your
credentials in your preferences for Dropbox files (configure access in
user preferences)."

**Affected Components**

- `lib/galaxy/files/sources/dropbox.py`: `_open_fs` turns the "OAuth2"
  error into this message.
- `lib/galaxy/files/sources/_fsspec.py` on `dev` and
  `lib/galaxy/files/sources/_pyfilesystem2.py` on `release_25.1`: the
  base classes, so a fix may differ by branch.
- `lib/galaxy/managers/remote_files.py`: the listing behind the API.

**Toolkit**

- Tool: Claude Code, reading the Dropbox code path on `dev` with me.
- What it produced: the file list above, and a note that the friendly
  message only covers an "OAuth2" error raised while connecting.
- Difficulties: reading the code couldn't say whether the no-token case
  reaches that message. Only running it could.
- What changed: I reproduced it on both branches instead of relying on
  the read.

---

## Reproduction Process

**Environment Setup**

- **OS:** Windows 11 Home, with WSL 2 running Ubuntu 24.04.4 LTS. Everything ran inside Ubuntu.
- **Tools:** Python 3.12.3, uv 0.13.7, git 2.56.0.
- **Galaxy `dev`:** commit `7e16f44` (version 27.0.dev0), with dropboxdrivefs 1.4.1 and dropbox 12.2.4.
- **Galaxy `release_25.1`:** commit `07d653f` (version 25.1.4.dev0), with fs.dropboxfs 1.0.9 and dropbox 12.2.4. This is the oldest supported release.
- **The issue's version:** the issue is from February 2023 and names no version, so these are much newer.
- **Setup:** I forked galaxy and cloned my fork to `~/galaxy`, then added `release_25.1` as a second folder:

  ```
  git clone https://github.com/Divergent-Code/galaxy.git ~/galaxy
  cd ~/galaxy
  git remote add upstream https://github.com/galaxyproject/galaxy.git
  git fetch upstream dev release_25.1
  git worktree add --detach ~/galaxy-25.1 upstream/release_25.1
  ```

- **Challenges:**
  - Galaxy's docs only cover Linux and macOS, and I'm on Windows, so I moved everything into WSL.
  - My first request used the wrong address (`dropbox://dropbox1/`). Its 400 error wasn't the bug. Galaxy's own plugin list gives the right one, `gxfiles://dropbox1`.
  - I skipped the web client build, so I tested the API only. I did not check the web page.

**Steps to Reproduce**

Run these from the top folder of each branch's copy (`~/galaxy`, then `~/galaxy-25.1`).

1. Create `config/file_sources_conf.yml`:

   ```yaml
   - type: dropbox
     id: dropbox1
     label: Dropbox files (configure access in user preferences)
     doc: Your Dropbox files - configure an access token via the user preferences
     access_token: ${user.preferences['dropbox|access_token']}
   ```

2. Create `config/user_preferences_extra_conf.yml`:

   ```yaml
   preferences:
       dropbox:
           description: Your Dropbox account
           inputs:
               - name: access_token
                 label: Dropbox access token
                 type: password
                 required: False
   ```

3. Start the server, with any string as the admin key, and wait for it to answer on port 8080:

   ```
   GALAXY_SKIP_CLIENT_BUILD=1 GALAXY_CONFIG_OVERRIDE_BOOTSTRAP_ADMIN_API_KEY=<admin key> sh run.sh
   ```

4. Create a test user and an API key for them. Never set a Dropbox token:

   ```
   curl -sS -X POST http://localhost:8080/api/users -H "x-api-key: <admin key>" -H "Content-Type: application/json" -d '{"username":"repro","email":"repro@example.org","password":"<any password>"}'
   curl -sS -X POST http://localhost:8080/api/users/<id from the last reply>/api_key -H "x-api-key: <admin key>"
   ```

5. Check the version, then list the Dropbox root as that user:

   ```
   curl -sS http://localhost:8080/api/version
   curl -sS -w '\nHTTP status: %{http_code}\n' "http://localhost:8080/api/remote_files?target=gxfiles://dropbox1/" -H "x-api-key: <user key>"
   ```

6. **Observed result:** see the evidence below. Expected, going by the issue: the raw `FileSourcePath(...)` error in its screenshot.

**Reproduction Evidence**

- **Commit showing reproduction:** none. I changed no code, only git-ignored config files.
- **Output on `dev` (`7e16f44`):**

  ```
  {"version_major":"27.0","version_minor":"dev0"}
  {"err_msg":"Permission Denied. Reason: OAuth2 access token or refresh token or app key/secret must be set. Please check your credentials in your preferences for Dropbox files (configure access in user preferences).","err_code":403001}
  HTTP status: 403
  ```

- **Output on `release_25.1` (`07d653f`):**

  ```
  {"version_major":"25.1","version_minor":"4.dev0"}
  {"err_msg":"Permission Denied. Reason: OAuth2 access token or refresh token or app key/secret must be set. Please check your credentials in your preferences for Dropbox files (configure access in user preferences).","err_code":403001}
  HTTP status: 403
  ```

- **My findings:** the raw object error is gone on both branches, and both return the same 403 message. That message still has jargon, and "Permission Denied" is odd for something that just isn't set up.
- **Limit:** the issue doesn't say how its Dropbox source was configured, so my setup may differ.
- **Findings comment:** https://github.com/galaxyproject/galaxy/issues/15551#issuecomment-6647261266

**Toolkit**

- Tool: repro-check, which graded my claim and repro report together
  before I moved the report here.
- What it produced: accept, with all six checks passing. A
  cannot-reproduce counts when the output is pasted.
- Difficulties: it can't read the screenshot, it couldn't find my
  scout's output in WSL, and it used a subagent to read
  `CONTRIBUTING.md`, which my rules don't allow unasked.
- What changed: nothing in the report. The screenshot text was checked
  separately in the browser.

---

## Solution Approach

**Analysis**

On `release_25.1`, listing Dropbox goes through `_open_fs` in
`lib/galaxy/files/sources/dropbox.py`, which calls
`DropboxFS(access_token=context.config.access_token)`. An empty token
fails inside the Dropbox library with `BadInputException`, and galaxy
repeats its error text: the `except` block finds "OAuth2" in the text
and wraps it in `AuthenticationRequired`. So the user sees it labelled
Permission Denied, when nothing was denied. It just isn't set up.

I checked this by resolving the file source directly on `release_25.1`
(`07d653f`), with the Dropbox preference empty, and calling `_open_fs`:

```
access_token repr: ''
_open_fs raised: AuthenticationRequired | Permission Denied. Reason: OAuth2 access token or refresh token or app key/secret must be set. Please check your credentials in your preferences for
caused by: dropbox.dropbox_client.BadInputException
```

The server log on `dev` (`7e16f44`) shows the same exception raised
under `DropboxDriveFileSystem(token=context.config.access_token)` in
`dropbox.py`:

```
dropbox.dropbox_client.BadInputException: OAuth2 access token or refresh token or app key/secret must be set
```

**Proposed Solution**

At the top of `_open_fs`, before anything connects, check for an empty
token and raise `AuthenticationRequired` with this message: "No Dropbox
access token is set. Add one in your user preferences." The status stays
403. I won't change the bad-token message, the base classes, other file
sources, or the web client.

**Unknowns**

- I've only tested through the API, not the web page.
- `dev` has different code here (`DropboxDriveFileSystem`), so merging a
  fix forward from `release_25.1` may need a separate change.
- No maintainer has answered whether a plainer message is still wanted.
  I'm following the issue's original suggestion unless one says
  otherwise.
- The test helper raises `KeyError` when the preference key is missing
  altogether, so the test sets it to empty. The real API case, where the
  user never set it, reached the same `BadInputException`.

**Implementation Plan**

1. Make the branch `fix/15551-dropbox-no-token-message` from
   `upstream/release_25.1`.
2. In `lib/galaxy/files/sources/dropbox.py`, add the empty-token check
   to `_open_fs`.
3. In `test/unit/files/test_dropbox.py`, add a test that lists Dropbox
   with an empty token and expects the new message. It needs no real
   token and no network. Run it without the fix to show it fails, then
   with the fix.
4. Re-run the reproduction steps through the API, and run `make
   diff-format`, `ruff check .` and the unit tests in
   `test/unit/files/`.

Deviations: I ran `isort --check-only` and `black --check` instead of
`make diff-format`, because that command changes files and I only wanted
to check them. I also added `flake8`, `run_tests.sh` and a check that a
set token still gets past the new code.

**Toolkit**

- Tool: plan-check, which graded `plan.md` and my draft comment against
  the issue and my repro report. I didn't post the comment.
- What it produced: reject on the first version, then accept, with all
  seven checks passing.
- Difficulties: it rejected the first version because the plan stated
  the token's value and the exception type as fact, and my repro only
  showed the final message.
- What changed: I called `_open_fs` directly, put that output in the
  Analysis, and added an Unknowns section.

---

## Testing Strategy

**Before and after.** I re-ran my reproduction's request, `GET
/api/remote_files?target=gxfiles://dropbox1/`, with no token set.

- Before, on `07d653f`: HTTP 403, "Permission Denied. Reason: OAuth2
  access token or refresh token or app key/secret must be set. Please
  check your credentials in your preferences for Dropbox files
  (configure access in user preferences)."
- After, on `754e2d6`: HTTP 403, "No Dropbox access token is set. Add
  one in your user preferences."

**New test.** `test_no_access_token_says_so` in
`test/unit/files/test_dropbox.py` needs no real token. Without the fix:
1 failed, 1 skipped. With the fix: 1 passed, 1 skipped.

**Repo checks, with the fix.**

- `pytest test/unit/files/`: 94 passed, 16 skipped.
- `bash run_tests.sh -u test/unit/files/test_dropbox.py`: 1 passed, 1
  skipped.
- `ruff check .`: all checks passed.
- `isort --check-only`, `black --check` and `flake8` on the two changed
  files: nothing flagged.
- A set token still gets past the new check: `_open_fs` returned
  `DropboxFS`.

**Still untested.** The web page, and a real bad token against Dropbox.

**Toolkit**

- Tool: pytest and galaxy's lint tools, run on my branch, with the
  before and after output saved in `test_evidence.md`.
- What it produced: the results above.
- Difficulties: the first failure wasn't the test: the runner stopped
  before running anything, so I read the output, not the exit code.
  Galaxy's existing Dropbox test also skips without a real token.
- What changed: I built a new environment for this branch, and wrote a
  test that needs no real token.

---

## Implementation Notes

**2026-10-28**

Went back to galaxy #15551 after someone opened a PR on NVDA #21022.
Read galaxy's rules: bug fixes go to the oldest supported release that
has the problem, not `dev`, and that's `release_25.1`.

**2026-11-04: reproduction**

Moved everything into WSL, because galaxy's docs only cover Linux and
macOS. The screenshot's error was gone on both branches, so I posted
that on the issue and chose a small wording fix instead of dropping the
PR.

**2026-11-04: the fix**

Built the empty-token check and its test as one commit on
`fix/15551-dropbox-no-token-message`. Ship-gate's installer needs a
normal clone, so I built in `~/galaxy`, not the second folder I used for
the repro.

Wrong turns: `run_tests.sh` failed with no pip, because `dev`'s
environment was built without it, so I built a new one. Moving the
`database` folder removed a tracked file. `git status` caught it, I
restored it, and it isn't in the commit.

Also updated the memory file's commands now that they've run. The Unit 6
entry above says they were marked not run, which was true at the time.

**Toolkit**

- Tool: Claude Code, which ran the commands in WSL while I chose the
  approach at each step.
- What it produced: the branch, the commit and the saved outputs.
- Difficulties: the Windows and WSL boundary. Git Bash rewrote a
  `/mnt/c` path once, and my findings comment went through Windows
  `claude`, not WSL's.
- What changed: those commands go through PowerShell or the Ubuntu
  window now.

---

## Pull Request

**PR Link:** https://github.com/galaxyproject/galaxy/pull/24755

**PR Description:** With no Dropbox token set, listing Dropbox returned "Permission Denied"
with OAuth2 wording. It now says no token is set and where to add one,
with the same 403 status. The description links the issue with `Fixes
#15551`, notes that the screenshot's original error no longer appears,
and shows the before and after output. It also names the cause, says
what isn't tested, and discloses AI use. Its testing section lists the
new test failing without the fix and passing with it, the file-source
tests, the lint and format checks, why `make diff-format` wasn't run,
and manual API steps.

**Maintainer Feedback:**

- 2026-11-05: none yet

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate, which runs my manifest's four checks and then my
  pr-precheck before `gh pr create`. I ran it by hand first, then opened
  the PR through Claude Code.
- What it produced: all four checks passed on both runs. My pr-precheck
  rejected the first run and passed the second.
- Difficulties: on the first run my own pr-precheck rejected the
  evidence. It didn't say why `make diff-format` was skipped, and flake8
  only said "flake8 done". Its installer also needs a normal clone, so
  my second galaxy folder wouldn't work.
- What changed: I showed each check's command and exit code and said why
  `make diff-format` wasn't run, and the second run passed. The hook
  then let the PR through on that pass without running again.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

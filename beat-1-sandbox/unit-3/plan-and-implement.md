# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

Divergent-Code

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/42#issuecomment-6024932151

Plan for #42, building on my reproduction above:

- **Change:** create `frontend/src/pages/__tests__/ReviewPage.test.tsx` with jest-axe tests for the review page's three states (loading, failed, and a completed review), each asserting `toHaveNoViolations()`. No other files change.
- **Test plan:** after the change, `npx vitest run` picks up the new file and its 3 tests pass. To show the tests can actually fail, I'll temporarily remove the Share button's text, expect a `button-name` violation, then revert.
- **Out of scope:** color contrast. jest-axe disables contrast rules under jsdom (`node_modules/jest-axe/index.js`, lines 55-56), so it would need a separate browser-based check. If the loading or failed states turn up a real violation, I'll report it here before changing any page markup.

For anyone following the thread: thanks to everyone looking into this, and it's good to see the missing test file and `src/pages/__tests__/` folder confirmed independently above. Two small notes from my repro that might save a step: `jest-axe` 8.0.0 is already listed in `frontend/package.json`, so no install is needed, and jest-axe turns its contrast rules off under jsdom, so contrast checks won't run in these tests.

---

## Your branch

**Branch**

`test/42-reviewpage-jest-axe`

**Evidence**

React Router "future flag" warning lines are left out of the outputs below; nothing else is removed. All "after" runs are on branch `test/42-reviewpage-jest-axe` at commit `644d6f8`.

**Before** (from my Unit 2 reproduction comment, fork at upstream `main` `f89c06f`):

```
$ cd frontend && npm install && npx vitest run
 ✓ src/components/__tests__/ReviewSection.test.tsx  (10 tests)
 ❯ src/components/__tests__/ProfileForm.test.tsx  (8 tests | 1 failed)
 Test Files  1 failed | 1 passed (2)
      Tests  1 failed | 17 passed (18)
```

```
$ git grep -n -i axe -- frontend/src
$ git ls-files frontend/src | grep test
frontend/src/components/__tests__/ProfileForm.test.tsx
frontend/src/components/__tests__/ReviewSection.test.tsx
frontend/src/test/setup.ts
```

**After**, step 1: the new test file.

```
$ npx vitest run src/pages/__tests__/ReviewPage.test.tsx
RUN  v1.6.1 C:/Users/onika/Programming/pathreview-ai301-fa26-s1/frontend

 ✓ src/pages/__tests__/ReviewPage.test.tsx  (3 tests) 282ms

 Test Files  1 passed (1)
      Tests  3 passed (3)
   Start at  16:40:07
   Duration  1.32s (transform 64ms, setup 185ms, collect 149ms, tests 282ms, environment 403ms, prepare 90ms)
```

**After**, step 2: the full suite. The one failure is the ProfileForm timeout that already failed before the change.

```
$ npx vitest run
RUN  v1.6.1 C:/Users/onika/Programming/pathreview-ai301-fa26-s1/frontend

 ✓ src/components/__tests__/ReviewSection.test.tsx  (10 tests) 611ms
 ✓ src/pages/__tests__/ReviewPage.test.tsx  (3 tests) 1205ms
 ❯ src/components/__tests__/ProfileForm.test.tsx  (8 tests | 1 failed) 6206ms
   ❯ src/components/__tests__/ProfileForm.test.tsx > ProfileForm > validates portfolio URL character limit
     → Test timed out in 5000ms.
If this is a long-running test, pass a timeout value as the last argument or configure it globally with "testTimeout".

⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/components/__tests__/ProfileForm.test.tsx > ProfileForm > validates portfolio URL character limit
Error: Test timed out in 5000ms.
If this is a long-running test, pass a timeout value as the last argument or configure it globally with "testTimeout".
⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed | 2 passed (3)
      Tests  1 failed | 20 passed (21)
   Start at  16:40:10
   Duration  8.16s (transform 197ms, setup 1.80s, collect 901ms, tests 8.02s, environment 1.63s, prepare 474ms)
```

**After**, step 3: jest-axe is now used in the frontend source.

```
$ git grep -n -i axe -- frontend/src
frontend/src/pages/__tests__/ReviewPage.test.tsx:4:import { axe, toHaveNoViolations } from 'jest-axe'
frontend/src/pages/__tests__/ReviewPage.test.tsx:9:// Register jest-axe's matcher with Vitest's types. The generic default
frontend/src/pages/__tests__/ReviewPage.test.tsx:64:// jest-axe disables color-contrast rules under jsdom, so these tests do not check contrast (see #42).
frontend/src/pages/__tests__/ReviewPage.test.tsx:71:  it('has no axe violations while the review is loading', async () => {
frontend/src/pages/__tests__/ReviewPage.test.tsx:76:    expect(await axe(container)).toHaveNoViolations()
frontend/src/pages/__tests__/ReviewPage.test.tsx:79:  it('has no axe violations when the review has failed', async () => {
frontend/src/pages/__tests__/ReviewPage.test.tsx:84:    expect(await axe(container)).toHaveNoViolations()
frontend/src/pages/__tests__/ReviewPage.test.tsx:87:  it('has no axe violations for a completed review', async () => {
frontend/src/pages/__tests__/ReviewPage.test.tsx:93:    expect(await axe(container)).toHaveNoViolations()
```

**After**, step 4: proving the tests can fail. I temporarily removed the visible text from the Share button in `ReviewPage.tsx`:

```
$ npx vitest run src/pages/__tests__/ReviewPage.test.tsx
RUN  v1.6.1 C:/Users/onika/Programming/pathreview-ai301-fa26-s1/frontend

 ❯ src/pages/__tests__/ReviewPage.test.tsx  (3 tests | 1 failed) 1420ms
   ❯ src/pages/__tests__/ReviewPage.test.tsx > ReviewPage accessibility > has no axe violations for a completed review
     → expect(received).toHaveNoViolations(expected)

Expected the HTML found at $('.border-gray-300') to have no violations:

<button class="inline-flex items-center gap-2 px-4 py-2 border border-gray-300 hover:bg-gray-50 text-gray-700 font-medium rounded-lg transition-colors">

Received:

"Buttons must have discernible text (button-name)"

Fix any of the following:
  Element does not have inner text that is visible to screen readers
  aria-label attribute does not exist or is empty
  aria-labelledby attribute does not exist, references elements that do not exist or references elements that are empty
  Element has no title attribute
  Element's default semantics were not overridden with role="none" or role="presentation"

You can find more information on this issue here: 
https://dequeuniversity.com/rules/axe/4.7/button-name?application=axeAPI

⎯⎯⎯⎯⎯⎯⎯ Failed Tests 1 ⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/pages/__tests__/ReviewPage.test.tsx > ReviewPage accessibility > has no axe violations for a completed review
Error: expect(received).toHaveNoViolations(expected)

Expected the HTML found at $('.border-gray-300') to have no violations:

<button class="inline-flex items-center gap-2 px-4 py-2 border border-gray-300 hover:bg-gray-50 text-gray-700 font-medium rounded-lg transition-colors">

Received:

"Buttons must have discernible text (button-name)"

Fix any of the following:
  Element does not have inner text that is visible to screen readers
  aria-label attribute does not exist or is empty
  aria-labelledby attribute does not exist, references elements that do not exist or references elements that are empty
  Element has no title attribute
  Element's default semantics were not overridden with role="none" or role="presentation"

You can find more information on this issue here: 
https://dequeuniversity.com/rules/axe/4.7/button-name?application=axeAPI
 ❯ src/pages/__tests__/ReviewPage.test.tsx:93:34
     91| 
     92|     expect(await screen.findByText('Portfolio Review')).toBeInTheDocum…
     93|     expect(await axe(container)).toHaveNoViolations()
       |                                  ^
     94|   })
     95| })

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/1]⎯

 Test Files  1 failed (1)
      Tests  1 failed | 2 passed (3)
   Start at  16:40:38
   Duration  5.19s (transform 249ms, setup 960ms, collect 635ms, tests 1.42s, environment 1.38s, prepare 301ms)
```

Then I reverted that edit (`git restore frontend/src/pages/ReviewPage.tsx`) and checked that the branch differs from `main` only by the new test file, and that the tests pass again:

```
$ git diff main --stat
frontend/src/pages/__tests__/ReviewPage.test.tsx | 95 ++++++++++++++++++++++++
 1 file changed, 95 insertions(+)
$ npx vitest run src/pages/__tests__/ReviewPage.test.tsx
RUN  v1.6.1 C:/Users/onika/Programming/pathreview-ai301-fa26-s1/frontend

 ✓ src/pages/__tests__/ReviewPage.test.tsx  (3 tests) 1155ms

 Test Files  1 passed (1)
      Tests  3 passed (3)
   Start at  16:40:57
   Duration  3.11s (transform 134ms, setup 281ms, collect 391ms, tests 1.16s, environment 648ms, prepare 200ms)
```

## Eval iterations

**Run history**

1. Smoke test (`--limit 3`): `agreement: 3/3 scored items`
2. Full run 1 (the committed `eval-run.txt`): `agreement: 19/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

My rubric rejected pkg-14 in full run 1 (`pkg-14  clear-accept       accept  reject   NO     failed: certainty-earned`); the gold label is accept.

The plan's diagnosis says the color-query responses leak on reattach, and grounds it in the repro: fresh attach is clean, the leak starts at reattach, and "0.44.1, which predates the reattach-path change in 0.44.2, is clean on the same setup." My `certainty-earned` check failed it on that phrase: the grader said it "states an unconfirmed code change as settled fact with no changelog/diff cited." The repro evidence does show a regression window (0.44.1 clean, "every reattach on 0.44.x from 0.44.2 on" leaks), so the phrase is an inference from that window rather than an invented fact, and the rest of the plan names its unknowns openly ("exact functions to be pinned in the PR", the Windows variant left out, the drain risk). My pass condition fails a plan for "a cause stated as found when the evidence only fits it", but it doesn't say how to weigh one inferred phrase against a plan that hedges everywhere else, so the grader read the single phrase as decisive. I kept this run as final rather than loosening the check. The check is about honesty, and in this eval set it never decided a correct rejection by itself, so loosening it would most likely have fixed pkg-14 without flipping anything else. But I couldn't confirm that without another full run, and the run already passed.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

`| certainty-earned | Every statement of cause, effort, or outcome in the plan and the plan comment, read against the repro evidence and the thread | Principle: the plan is only as certain as its evidence. Fail if the plan or comment states as settled something the evidence does not establish (a cause stated as found when the evidence only fits it, an effort or time estimate stated as fact, a fix promised as certain), or leaves out an unknown the evidence plainly leaves open. Pass if remaining unknowns and risks are named as unknowns. | required |`

I wrote this check from the principle that a plan is only as certain as its evidence, after the practice packages showed plans claiming more than they had: calib-02's "should be doable in a few evenings" and calib-03's "I traced this to" beside evidence that pointed somewhere else. I rejected the alternative, "pass if the plan has a risks section", because it judges the plan's shape: a confident, wrong plan with one token risk line would pass. The Notes section marks "found versus fits the evidence" as a judgment call.

**Trade-offs**

This check gives up pkg-14, a clear-accept plan it rejected over one inferred phrase ("0.44.1, which predates the reattach-path change in 0.44.2"), even though the rest of the plan names its unknowns. Across full run 1 it failed 13 packages, and pkg-14 is the only one it failed alone. Every other package it failed also failed another required check, so in this eval set it decided no correct rejection by itself. What it would catch on its own is a plan that is well-grounded, bounded and testable, but oversells its certainty. This set has no package like that, so that benefit is unproven here. I accepted the pkg-14 miss rather than spend another full run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

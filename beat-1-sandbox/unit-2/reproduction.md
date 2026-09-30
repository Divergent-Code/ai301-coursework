# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

---

## Your identity upstream

**GitHub username**

Divergent-Code

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/42#issuecomment-5920080243

Hi! I'd like to work on this as my first Path Review contribution: adding `jest-axe` checks to `frontend/src/pages/__tests__/ReviewPage.test.tsx` so the review page is tested for the violations listed here (contrast, missing labels, heading structure).

Next I'll set up the frontend from the repo's docs, run the existing ReviewPage tests, and confirm what accessibility coverage they have today. I'll also check which of the three listed checks `jest-axe` can actually run in the test environment. I'll post a reproduction report here with the commands and output.

I work with AI assistance (Claude) and verify every command and output myself.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/42#issuecomment-5920206498

Reproduction report for #42.

**Environment:** Windows 11, Node v24.15.0, npm 11.14.1 (SETUP.md asks for Node 18+ and npm 9+). My fork at upstream `main` commit `f89c06f`. Frontend test stack as installed: vitest 1.6.1, jsdom 23.2.0, jest-axe 8.0.0 (bundles axe-core 4.7.2).

**Steps and observed:**

1. Install and run the existing frontend tests:

```
$ cd frontend && npm install && npx vitest run
 ✓ src/components/__tests__/ReviewSection.test.tsx  (10 tests)
 ❯ src/components/__tests__/ProfileForm.test.tsx  (8 tests | 1 failed)
 Test Files  1 failed | 1 passed (2)
      Tests  1 failed | 17 passed (18)
```

Only two test files exist, and neither covers the review page. (The one failure is `ProfileForm > validates portfolio URL character limit`, which times out at 5000ms on every run. It fails before any change and is unrelated to this issue.)

2. Look for any accessibility testing in the frontend source:

```
$ git grep -n -i axe -- frontend/src
$ git ls-files frontend/src | grep test
frontend/src/components/__tests__/ProfileForm.test.tsx
frontend/src/components/__tests__/ReviewSection.test.tsx
frontend/src/test/setup.ts
```

`git grep` returns nothing: `jest-axe` is installed in `package.json` but not used anywhere, and `frontend/src/pages/__tests__/ReviewPage.test.tsx` (named in the issue) does not exist yet, so this work means creating it.

3. To see which of the three listed checks `jest-axe` can run here, I rendered the completed-review state of `ReviewPage` in a temporary test (mocked `useReviewStatus` and `apiClient.getReview`, not committed; code below) and ran axe on it:

```
$ npx vitest run src/__probe__
PROBE axe-core version: 4.7.2
PROBE violations: []
PROBE rule color-contrast: not run
PROBE rule label: inapplicable
PROBE rule button-name: pass
PROBE rule heading-order: pass
PROBE rule page-has-heading-one: inapplicable
```

`color-contrast` is "not run" because jest-axe disables color rules by default. From `node_modules/jest-axe/index.js`, lines 55-56: "Color contrast checking doesnt work in a jsdom environment. So we need to identify them and disable them by default."

**Expected:** the review page has jest-axe tests covering contrast, labels, and heading structure.

**Actual:** there are no accessibility tests for the review page (step 2). Of the three checks the issue lists, heading structure and labels (`heading-order`, `label`, `button-name`) can run under jsdom, and found no violations in the completed-review state I rendered. Contrast cannot be checked by jest-axe in this environment (step 3), so it would need a different approach, such as a browser-based check.

<details>
<summary>Temporary probe used in step 3 (src/__probe__/ReviewPage.axe.probe.test.tsx)</summary>

```tsx
import React from 'react'
import { render, screen } from '@testing-library/react'
import { MemoryRouter, Route, Routes } from 'react-router-dom'
import { vi } from 'vitest'
import { axe } from 'jest-axe'

const { review } = vi.hoisted(() => ({
  review: {
    id: 'r1',
    status: 'complete',
    overall_score: 0.72,
    sections: [
      { section_name: 'Projects', confidence: 0.9, content: 'Solid projects.', suggestions: ['Add tests'] },
    ],
  },
}))

vi.mock('../hooks/useReviewStatus', () => ({
  useReviewStatus: () => ({ review, isPolling: false, error: '' }),
}))
vi.mock('../services/api', () => ({
  apiClient: { getReview: vi.fn().mockResolvedValue(review) },
}))

import { ReviewPage } from '../pages/ReviewPage'

it('probe: run axe on the completed ReviewPage', async () => {
  const { container } = render(
    <MemoryRouter initialEntries={['/reviews/r1']}>
      <Routes>
        <Route path="/reviews/:reviewId" element={<ReviewPage />} />
      </Routes>
    </MemoryRouter>
  )
  await screen.findByText('Portfolio Review')
  const results = await axe(container)
  const find = (id: string) =>
    results.passes.some((r) => r.id === id) ? 'pass'
    : results.violations.some((r) => r.id === id) ? 'VIOLATION'
    : results.incomplete.some((r) => r.id === id) ? 'incomplete (could not decide)'
    : results.inapplicable.some((r) => r.id === id) ? 'inapplicable'
    : 'not run'
  console.log('PROBE axe-core version:', (results as any).testEngine?.version)
  console.log('PROBE violations:', JSON.stringify(results.violations.map((v) => ({ id: v.id, nodes: v.nodes.length }))))
  for (const id of ['color-contrast', 'label', 'button-name', 'heading-order', 'page-has-heading-one']) {
    console.log(`PROBE rule ${id}: ${find(id)}`)
  }
})
```

</details>

## Eval iterations

**Run history**

1. Smoke test (`--limit 3`): `agreement: 2/3 scored items`
2. Canary check after loosening `claims-backed` (`--only pkg-03,calib-02,calib-03 --include-calibration`): `agreement: 1/1 scored items` (calib-02 and calib-03 are unscored; both still rejected)
3. Full run 1: `agreement: 19/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`
4. Canary check after changing `follows-policy` (`--only pkg-20,pkg-09,pkg-03,pkg-05,pkg-07,pkg-12`): `agreement: 6/6 scored items`
5. Full run 2 (the committed `eval-run.txt`): `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

My rubric accepted pkg-20 in full run 1 (`pkg-20  reject  accept   NO     graded accept`); the gold label is reject. In full run 2 it rejects it, matching the gold label.

The repo's policy requires that all AI usage be disclosed with the tool and extent, and neither the claim comment nor the repro report mentions AI. In run 1 the grader passed `follows-policy` with "nothing in either draft indicates AI was used, so no disclosure is owed." My check only said a requirement had to be met, so the grader treated silence as evidence that no AI was used. But work in this course is AI-assisted, so silence means the requirement was broken. I rewrote the check so a requirement to state something passes only if the statement appears. In run 2 the grader's evidence is "neither the claim comment nor the repro report contains any such disclosure." pkg-20 is the only disclosure package, so this one miss was also the category floor (`disclosure 0/1` in run 1, `disclosure 1/1` in run 2).

**Check rationale**

Quoted from `tools/repro-check/rubric.md`:

`| follows-policy | The contribution policy line in repo facts (including any AI-use disclosure requirement), read against the claim comment and repro report | Principle: the words follow the repo's stated rules, and work in this course is AI-assisted. A requirement applies only where the policy says it applies: one aimed only at pull requests or code does not apply to these comments. A requirement to state something that does apply to comments or to all contributions (for example, disclose AI use, the tool, or its extent) passes only if that statement appears in the claim comment or repro report; its absence is a fail, not evidence that no AI was used. A requirement about how the text was produced (for example, "written by a human in their own words") passes unless something in the comments contradicts it. A policy with no requirement on comments passes. | required |`

In full run 1 this check only said a policy requirement had to be met, and the grader passed pkg-20 because "nothing in either draft indicates AI was used." I added the principle that work in this course is AI-assisted, so a missing disclosure is a fail. I rejected a broader rule, "every requirement must be visibly met", because pkg-03's policy says comments "must be written by humans in their own words", which no comment can show, so that rule would have rejected an accepted package. That is why the check separates requirements to state something from requirements about how the text was produced. Before re-running, I also found pkg-09's policy asks for disclosure "in the pull request" and "states no disclosure ask for issue comments", so I added that a requirement applies only where the policy says it applies.

**Trade-offs**

Tightening this check risked rejecting good packages whose repos have AI policies. Before the confirming full run I re-ran `--only pkg-20,pkg-09,pkg-03,pkg-05,pkg-07,pkg-12`: pkg-20 flipped to reject and the five accepted packages with AI policies stayed accept (`agreement: 6/6 scored items`). In full run 2 the only row that changed from run 1 was pkg-20. What the check gives up: sorting a requirement into "state something" or "how it was produced" is a judgment call when one policy sentence mixes both, and the grader could sort it differently on another run. It also assumes AI-assisted work, so it would wrongly fail a genuinely unassisted contributor who stays silent under a disclosure policy.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

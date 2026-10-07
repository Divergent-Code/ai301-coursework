# Plan for #42: jest-axe accessibility tests for the review page

## Diagnosis

The review page has no automated accessibility checks because no test exercises it, not because of a bug in the page itself. From my reproduction on this issue (fork at upstream `main` `f89c06f`, which upstream has not changed since):

- `git grep -n -i axe -- frontend/src` returns nothing, and `git ls-files frontend/src | grep test` lists only `ProfileForm.test.tsx`, `ReviewSection.test.tsx`, and `test/setup.ts`. So `frontend/src/pages/__tests__/ReviewPage.test.tsx`, named in the issue, does not exist yet.
- `jest-axe` 8.0.0 is already a dependency in `frontend/package.json` (bundling axe-core 4.7.2), but nothing imports it, and `src/test/setup.ts` does not register its `toHaveNoViolations` matcher.
- A temporary probe rendering the completed-review state found `violations: []`, with `heading-order` and `button-name` passing and `label` inapplicable. `color-contrast` was "not run": jest-axe disables color rules by default because "Color contrast checking doesnt work in a jsdom environment" (`node_modules/jest-axe/index.js`, lines 55-56).

So the missing piece is test coverage: a test file that renders the page and asserts axe finds no violations.

## Scope

In scope:
- Create `frontend/src/pages/__tests__/ReviewPage.test.tsx` with jest-axe tests for the three states the page renders: loading (polling), failed review, and completed review with feedback sections.
- Register `toHaveNoViolations` inside that test file, so the change stays in one new file.

Not in scope:
- **Color contrast.** jest-axe cannot check it under jsdom (see Diagnosis). A browser-based check would be a separate change with new tooling.
- Changing `ReviewPage.tsx` markup. If a test finds a real violation in the loading or failed state, I'll record it under Deviations and ask on the thread before touching page code.
- The pre-existing timeout in `ProfileForm > validates portfolio URL character limit`, and the `npm audit` warnings. Both exist before any change and are unrelated.

## Files

- `frontend/src/pages/__tests__/ReviewPage.test.tsx` (new). No other files change.

## Approach

1. Create the test file in the style of `src/components/__tests__/ReviewSection.test.tsx`: explicit `describe`/`it`/`expect`/`vi` imports from vitest, and typed fixtures using `Review` and `FeedbackSection` from `../../types`.
2. Import `axe` and `toHaveNoViolations` from `jest-axe` and call `expect.extend({ toHaveNoViolations })`.
3. Mock `useReviewStatus` and `apiClient.getReview` with `vi.mock`, using `vi.hoisted` so the fixtures exist when the mocks are hoisted. Each test sets the hook's return value for its state.
4. Add a small `renderReviewPage()` helper that renders `ReviewPage` inside a `MemoryRouter` at `/reviews/:reviewId`.
5. Write three tests, one per state. Each waits for that state's visible text ("Analyzing your portfolio...", "Review Failed", "Portfolio Review"), runs `axe(container)`, and asserts `expect(results).toHaveNoViolations()`.
6. Add a one-line comment in the file noting that color-contrast rules are disabled by jest-axe under jsdom.

## Test plan

Before (from my reproduction):

```
$ git grep -n -i axe -- frontend/src
(no output)
$ npx vitest run
 Test Files  1 failed | 1 passed (2)
      Tests  1 failed | 17 passed (18)
```

After the change, expected:

1. `npx vitest run src/pages/__tests__/ReviewPage.test.tsx` runs 3 tests, and all 3 pass.
2. `npx vitest run` runs 3 test files and 21 tests. The only failure is the pre-existing ProfileForm timeout.
3. `git grep -n -i axe -- frontend/src` now lists the new test file.
4. **The tests can fail.** As a temporary check, I'll remove the visible text from the Share button in `ReviewPage.tsx`, re-run the test file, and expect the completed-state test to fail with a `button-name` violation. Then I'll revert that edit and confirm `git diff` shows only the new test file.

## Risks and unknowns

- I only probed the completed state. The loading and failed states may have violations I haven't seen; that would be a finding, handled as described in Scope.
- The `lucide-react` icons render as SVGs, and I don't yet know how axe treats them in each state.
- React Router prints "future flag" warnings during these tests. They are warnings, not failures, but they add noise to the output.

## Deviations

1. **Added a type declaration to the test file.** I added a 6-line `declare module 'vitest'` block so TypeScript knows jest-axe's `toHaveNoViolations` matcher. The frontend's `tsconfig.json` includes all of `src`, so `npm run build` (`tsc && vite build`) type-checks test files, and without the declaration that type check would fail. The change is still one new file, and no other files changed.
2. **The loading and failed states had no violations.** The risk I listed didn't happen, so no page markup changed and there was nothing to report on the thread.

Everything else ran as planned. The test plan results matched the expected ones, including the Share-button check, which failed on `button-name` as intended and was then reverted.

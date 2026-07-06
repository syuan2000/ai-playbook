# Regression Test Workflow

> Make sure the thing that worked yesterday still works today.

**Use when:** before a release, after a refactor, after a bugfix, or any time a
change touches shared/core code. Pairs with [bugfix.md](./bugfix.md),
[refactor.md](./refactor.md), and [release.md](./release.md).

**The core idea:** a regression is old behavior that silently broke. You can't
manually re-test everything on every change, so you build a small, growing safety
net that re-tests the important stuff *for* you.

**Solo-dev reality:** you don't need a QA team or a 500-case matrix. You need
(1) a short list of critical paths and (2) the discipline to add a test every time
something breaks. That's 90% of the value.

---

## 1. Know your critical paths (write them down once)

List the handful of journeys that **must never break**. For most apps that's 3–7:

- The main thing the app is for (e.g. "create a note and it persists").
- Auth: sign up, log in, log out.
- Anything involving money, data loss, or security.
- The flow you changed most recently.

Keep this list in the repo (e.g. `workflows/critical-paths.md` or a test file
header). It's your manual smoke-test script *and* the priority order for automated
tests.

## 2. Grow the suite from real breakage (the golden rule)

**Every bug fixed = one regression test added.** This is the single highest-value
habit here (see [bugfix.md](./bugfix.md) step 5).

- The test must **fail before the fix, pass after**. That proves it actually
  guards the bug.
- Name it so future-you knows what it protects:
  `test_cart_total_with_no_items_does_not_crash`.
- Over a few months this suite becomes the map of every way your app has ever
  broken — exactly the things most likely to break again.

## 3. Pick the cheapest test that catches the regression

Don't gold-plate. Match the tool to the risk:

- **Unit test** — pure logic, calculations, edge cases. Fast, cheap, most of your
  suite.
- **Integration test** — a few pieces together (API + DB, component + store).
  For the seams where regressions love to hide.
- **Snapshot / golden test** — output that shouldn't change unexpectedly (rendered
  HTML, API JSON, generated files). Cheap to add; review diffs before accepting a
  new snapshot so you don't bless a real regression.
- **End-to-end** — reserve for your 1–3 truly critical paths only. Slow and
  flaky; a little goes a long way.
- **Manual smoke test** — always fine as a backstop when automating is expensive.
  Just run your critical-paths list by hand.

## 4. Run regression at the right moments

- **After a refactor:** the whole point is behavior is unchanged — the suite is
  your proof ([refactor.md](./refactor.md) step 5).
- **After a bugfix:** run the surrounding area, not just the new test.
- **Before a release:** full suite green + manual smoke of critical paths
  ([release.md](./release.md) step 1).
- **Ideally on every push:** wire the suite into CI so you can't forget. Even a
  single "run tests on push" workflow is worth setting up once.

## 5. When a regression slips through anyway

- **Reproduce it, then bisect.** `git bisect` finds the exact commit that broke it
  fast — let it do the work.
- Fix it via [bugfix.md](./bugfix.md), and add the regression test that would have
  caught it. The net just got one hole smaller.
- If a test breaks *legitimately* (behavior intentionally changed), update the test
  in the same commit as the change — never comment it out "for now."

## 6. Keep the suite trustworthy

A suite you don't trust is worse than none — you'll start ignoring red.

- **Kill flaky tests.** A test that fails randomly trains you to ignore failures.
  Fix it or delete it.
- **Keep it fast.** If the suite takes too long, you'll skip it. Push slow tests to
  CI, keep the local run snappy.
- **Delete tests for deleted features.** Dead tests are noise.

---

## Quick checklist

- [ ] Critical paths listed and kept in the repo
- [ ] Every fixed bug got a regression test (fails before, passes after)
- [ ] Cheapest test type chosen for each case (not over-engineered)
- [ ] Suite run after refactors, bugfixes, and before releases
- [ ] Tests wired into CI to run automatically
- [ ] Regressions traced with `git bisect`, then covered by a new test
- [ ] Intentional behavior changes update tests in the same commit
- [ ] No flaky or dead tests left rotting in the suite

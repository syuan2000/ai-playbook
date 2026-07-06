# Feature Workflow

> Build a new feature from idea to shipped, wearing every hat yourself.

**Use when:** you're adding something new. If you're fixing broken behavior, use
[bugfix.md](./bugfix.md). If you're restructuring code that already works, use
[refactor.md](./refactor.md).

**Rule of thumb:** the smaller the slice you can ship, the better. Prefer 3 small
features over 1 big one.

---

## 1. Define it (PM hat) — 5 min, don't skip

Answer these in a sentence each. If you can't, you're not ready to build.

- **Problem:** what can't I / the user do today?
- **Outcome:** what's true after this ships? (one sentence)
- **Scope:** the smallest version worth shipping. Write down what's explicitly
  *out* — this is where scope creep dies.
- **Done looks like:** 1–3 concrete acceptance checks (e.g. "user can log in with
  email and stays logged in after refresh").

> Skip formal user stories / PRDs. One short paragraph in the PR description or an
> issue is enough for a solo project.

## 2. Sketch it (Designer hat) — only if there's UI

- Rough the UI on paper / whiteboard / Figma. Don't build pixel-perfect yet.
- Decide the states up front: **empty, loading, error, success**. Most bugs live
  in the states you forgot.
- Reuse existing components/patterns before inventing new ones.
- Full UI passes go in [design.md](./design.md).

## 3. Plan the build (Engineer hat) — 10 min

- Find the 1–3 files you'll actually touch. Read them first.
- Data first: what shape does the data have? Model/types before UI.
- Name the seams: API contract, function signatures, component props. Agree with
  yourself on these before writing bodies.
Before implementing:

- Does a similar feature already exist?
- Does this fit the current architecture?
- Can I reuse existing components?
- Am I introducing a new pattern or following an existing one?

Prefer consistency over cleverness.
## 4. Build it

- Don't optimize early. Only optimize when: there is measurable slowdown, the feature renders large datasets, the user experience noticeably suffers
- Commit in small logical chunks with clear messages. One concern per commit.
- Handle the error and empty states you listed in step 2 — now, not "later".
- Leave `TODO(name): why` for deliberate shortcuts so they're findable.

## 5. Verify it (QA hat)

- Walk through each acceptance check from step 1 manually.
- Try to break it: empty input, huge input, double-click, no network, wrong types.
- Add tests for the core logic and the one bug you're most afraid of. You don't
  need 100% coverage on a personal project — cover what would hurt to break.
- Run the app for real, not just the tests.

## 6. Review it (Reviewer hat)

- Read your own diff top to bottom before committing. See [review.md](./review.md).
- Ask: would this confuse me in 6 months? If yes, add a comment or rename.
- Remove debug logs, commented-out code, and dead branches.

## 7. Ship it

- Squash-merge (or clean history) → deploy. See [release.md](./release.md) for the
  pre-deploy checks.
- Watch it work in production once. Then close the loop and move on.

---

## Quick checklist

- [ ] Problem + outcome + scope written down (1 sentence each)
- [ ] Out-of-scope explicitly listed
- [ ] Acceptance checks defined
- [ ] UI states covered (empty/loading/error/success) — if applicable
- [ ] Data model / types decided before UI
- [ ] Error + empty states handled in code
- [ ] Acceptance checks pass manually
- [ ] Tests for core logic + scariest bug
- [ ] Self-reviewed the diff
- [ ] Shipped and verified in prod

# Release Workflow

> A short pre-deploy checklist so you don't ship a broken build at 11pm.

**Use when:** about to deploy / publish. Keep it lightweight — this is a personal
project, not a change-approval board. The point is to catch the dumb, avoidable
breakages.

**Mindset:** ship small and often. Small releases are easy to verify and easy to
roll back. Big-bang releases are where weekends go to die.

---

## 1. Before you deploy

- [ ] On the right branch, latest `main` pulled/merged in.
- [ ] `main` is green: build passes, tests pass, linter/typecheck clean.
- [ ] Self-reviewed the diff since last release ([review.md](./review.md)).
- [ ] No secrets or debug code in the diff.
- [ ] Env vars / config for the target environment are set (and documented).
- [ ] Ran the app locally against a production-like build once.
- [ ] DB migrations (if any) are backward-compatible and tested on a copy.

## 2. Deploy

- [ ] Tag or note the version so you know what's live (`v0.3.1`, or a commit SHA).
- [ ] Deploy. Prefer a method you can undo (previous build kept, one-click rollback).
- [ ] Have the rollback command ready *before* you push the button.

## 3. After you deploy (verify — don't assume)

- [ ] Load the live app. Does it actually come up?
- [ ] Smoke-test the critical path (log in, the main action, the thing you changed).
- [ ] Check logs/errors for the first few minutes.
- [ ] Confirm the specific thing this release shipped is working in prod.

## 4. If it's broken

- [ ] **Roll back first, debug second.** Don't fix-forward under pressure.
- [ ] Reproduce the failure locally, then use [bugfix.md](./bugfix.md).
- [ ] Note what broke so the checklist above can grow a line to catch it next time.

## 5. Wrap up

- [ ] Jot a one-line changelog / release note (even just in the git tag).
- [ ] Close the issues/PRs this release resolved.

---

## Minimal version (for tiny projects)

If the full list is overkill for a weekend project, never skip these four:

1. Tests + build green on `main`.
2. No secrets in the diff.
3. Verify the live app loads and the changed thing works.
4. Know how to roll back.

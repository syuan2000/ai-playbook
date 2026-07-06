# Bugfix Workflow

> Reproduce → understand → fix the root cause → prove it's gone.

**Use when:** something that should work doesn't. If the code works but is ugly,
that's [refactor.md](./refactor.md), not a bug.

**Golden rule:** don't fix anything you can't reproduce, and don't stop at the
first symptom. Fix the *cause*, not the *symptom*.

---

## 1. Reproduce it first

- Get a reliable repro: exact steps, inputs, environment. If it's intermittent,
  find what makes it more likely.
- Write the repro down (even one line). This becomes your test later.
- If you can't reproduce it, don't guess-fix. Add logging and wait for it to
  happen again.

## 2. Understand it (don't skip to the fix)

- Read the actual error + stack trace. Fully. The answer is often right there.
- Bisect: when did it last work? `git log` / `git bisect` on the suspect area.
- Form one hypothesis: "I think X because Y." Then prove or kill it.
- Add a breakpoint / log at the boundary between "state is good" and "state is
  bad". Narrow the gap until you find the exact line.

## 3. Find the root cause

Ask **"why"** until you hit something real:

- The crash is a symptom. Why did the value get there? Why wasn't it validated?
- Is this bug's cousin lurking elsewhere? (Same pattern, other file.)
- Decide: fix the root cause, or consciously patch the symptom with a
  `TODO(name): root cause is X` note. Don't patch by accident.

## 4. Fix it

- Smallest change that addresses the cause. Resist "while I'm here" refactors —
  do those separately (see [refactor.md](./refactor.md)).
- If the fix touches shared code, check the other callers.

## 5. Prove it's fixed

- **Write a test that fails before your fix and passes after.** This is the one
  place regression tests really pay off — the bug already proved it can happen.
- Re-run your step-1 repro manually. Gone?
- Check you didn't break the neighbors: run the surrounding tests / flows.

## 6. Close it out

- Commit message says *what was broken and why*, not just "fix bug".
  e.g. `fix: prevent crash when cart is empty (null total)` .
- If it was a nasty one, drop a one-line comment at the fix site so future-you
  doesn't "clean it up" and reintroduce it.

---

## Quick checklist

- [ ] Reliable reproduction found and written down
- [ ] Read the full error + stack trace
- [ ] Root cause identified (not just the symptom)
- [ ] Checked for the same bug elsewhere
- [ ] Smallest fix that addresses the cause
- [ ] Regression test: fails before, passes after
- [ ] Repro re-run manually — confirmed gone
- [ ] Surrounding tests/flows still pass
- [ ] Commit explains what was broken and why

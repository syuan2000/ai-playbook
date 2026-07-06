# Review Workflow

> Read your own diff like a skeptical stranger before it merges.

**Use when:** before every merge/deploy, even solo. Reviewing your own code an hour
later catches an surprising amount. Pairs with [release.md](./release.md).

**How to self-review well:** step away for a bit, then read the **whole diff top to
bottom** — not the code you remember writing, the code that's actually there.

---

## Correctness (most important)

- [ ] Does it actually do what the task asked? Re-read the goal, then the diff.
- [ ] Edge cases: empty, null/undefined, zero, negative, very large, duplicates.
- [ ] Error handling: what happens when the call fails / network is down?
- [ ] Off-by-one, boundary conditions, and inverted logic (`&&` vs `||`, `!`).
- [ ] Async: awaited promises, no race conditions, no unhandled rejections.

## Security & data (quick but non-negotiable)

- [ ] No secrets, API keys, or tokens committed. (Check `.env` is gitignored.)
- [ ] User input is validated/sanitized before use (queries, HTML, file paths).
- [ ] No obvious injection (SQL/command/XSS) on untrusted input.
- [ ] Right data exposed to the right user — no leaking others' data.

## Readability & maintenance

- [ ] Names say what things are. Would this confuse me in 6 months?
- [ ] No commented-out code, debug logs, or `console.log` left behind.
- [ ] No copy-paste duplication that should be one function.
- [ ] Comments explain *why*, not *what*. Delete comments that just restate code.
- [ ] Functions do one thing; nesting isn't ridiculous.

## Scope & hygiene

- [ ] The diff only contains what this change needs — no unrelated edits.
- [ ] No new dependency added for something trivial you could write in 10 lines.
- [ ] Tests updated/added for the behavior that changed.
- [ ] TODOs are intentional and attributed: `TODO(name): why`.

---

## When reviewing someone else's PR (or an AI's)

- Be specific and kind: point at the line, suggest the fix.
- Separate **must-fix** (bugs, security) from **nice-to-have** (style, taste).
  Don't block a merge on taste.
- Ask questions instead of decreeing: "what happens here if `x` is null?"
- Pull the branch and run it for anything non-trivial. Reading isn't running.

> For a deeper automated pass on your own working diff, the repo's `/code-review`
> skill reviews the current diff for correctness and cleanup at a chosen effort
> level.

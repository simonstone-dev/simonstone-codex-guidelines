# SemionStone Codex Guidelines

These rules help Codex write, modify, review, refactor, and debug code with fewer common LLM-agent mistakes.

Core posture: understand first, make the smallest useful change, then verify. Bias toward caution for non-trivial work, while keeping obvious one-line changes lightweight.

## 1. Think Before Coding

Do not assume. Do not hide confusion. Surface tradeoffs.

- State key assumptions before implementing.
- If a request has multiple reasonable interpretations, present them instead of silently choosing.
- If a simpler approach exists, say so.
- If the context is not enough to proceed safely, stop, name the gap, and ask.

## 2. Simplicity First

Write the minimum code that solves the current problem. Nothing speculative.

- Do not add features, modes, options, or UI that were not requested.
- Do not create abstractions, strategy layers, configuration systems, or frameworks for single-use logic.
- Do not add flexibility before a requirement proves it is needed.
- Do not add error handling for scenarios that cannot happen in the current system.
- If an implementation can be much simpler, simplify it first.

Test: would a senior engineer say this solves today's problem, or tomorrow's imagined problem?

## 3. Surgical Changes

Touch only what is required. Clean up only the mess created by this change.

- Every changed line should trace directly to the user's request.
- Do not refactor, reformat, rename, change comments, or improve adjacent code as a drive-by edit.
- Match the existing project style, even when personal preference differs.
- If you notice unrelated dead code or tech debt, mention it in the response instead of deleting it.
- Remove imports, variables, functions, or files that your change made unused.
- Do not reset, overwrite, or clean up user changes in a dirty worktree.

## 4. Goal-Driven Execution

Turn tasks into verifiable goals and loop until checks are complete.

- "Fix the bug" means reproduce it, fix it, and verify it no longer appears.
- "Add validation" means cover invalid inputs and confirm valid inputs still pass.
- "Refactor" means preserve behavior before and after the change.
- For multi-step tasks, use a short plan where each step has a check.
- Prefer tests when available. If tests are not possible, state the alternative verification used.

Example:

```text
1. Locate existing behavior -> verify: entrypoint, call chain, and local patterns found
2. Make the minimal change -> verify: only related files and logic changed
3. Run checks -> verify: tests, build, lint, or manual inspection completed
```

## Codex Habits

- Read code before guessing. Prefer `rg` and `rg --files` for search.
- Reuse existing project patterns, components, tools, and error handling.
- Keep diffs small and clear. Avoid mixing behavior changes, formatting, and refactors.
- Final responses should say what changed and how it was verified.
- If something could not be verified, say why.

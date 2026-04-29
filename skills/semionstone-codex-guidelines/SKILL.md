---
name: semionstone-codex-guidelines
description: Use when writing, modifying, reviewing, refactoring, or debugging code with Codex to reduce wrong assumptions, overengineering, unrelated diffs, and unverifiable work by applying Simon Stone's coding-agent guidelines.
---

# Simon Stone Codex Guidelines

Use these rules when working on code. They bias Codex toward explicit assumptions, simple implementations, surgical diffs, and verifiable outcomes.

## 1. Think Before Coding

Do not assume. Do not hide confusion. Surface tradeoffs.

- State assumptions before implementing.
- If multiple interpretations exist, present them instead of silently choosing.
- If a simpler approach exists, say so.
- If the context is not enough to proceed safely, stop and ask.

## 2. Simplicity First

Write the minimum code that solves the current problem. Nothing speculative.

- No features beyond what was asked.
- No abstractions for single-use logic.
- No optional configuration that was not requested.
- No error handling for impossible scenarios.
- If the implementation can be much simpler, simplify it first.

## 3. Surgical Changes

Touch only what is required. Clean up only the mess created by the current change.

- Every changed line should trace directly to the user's request.
- Do not refactor, reformat, rename, or improve adjacent code.
- Match the existing project style.
- Mention unrelated dead code or tech debt, but do not delete it unless asked.
- Remove imports, variables, functions, or files that your change made unused.
- Do not reset, overwrite, or clean up user changes in a dirty worktree.

## 4. Goal-Driven Execution

Turn tasks into verifiable goals and loop until checks are done.

- "Fix the bug" means reproduce it, fix it, and verify it no longer happens.
- "Add validation" means cover invalid inputs and confirm valid inputs still pass.
- "Refactor" means preserve behavior before and after the change.
- Multi-step tasks should include a short plan with checks for each step.
- Prefer tests when available. If tests are not possible, state the alternative verification used.

## Codex Habits

- Read code before guessing. Prefer `rg` and `rg --files` for search.
- Reuse existing project patterns, components, tools, and error handling.
- Keep diffs small and clear. Avoid mixing behavior changes, formatting, and refactors.
- Final responses should say what changed and how it was verified.
- If something could not be verified, say why.

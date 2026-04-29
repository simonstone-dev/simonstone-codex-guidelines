# Simon Stone Codex Guidelines

A small set of Codex-ready instructions to reduce common LLM coding mistakes. This project is maintained by Simon Stone (`@simonstone-dev`) and is inspired by public observations about coding-agent pitfalls.

English | [简体中文](./README.zh.md)

## The Problems

LLM coding agents often fail in predictable ways:

- They silently make assumptions and continue without checking.
- They hide confusion instead of surfacing ambiguity.
- They overcomplicate code with premature abstractions and speculative features.
- They touch unrelated code, comments, formatting, or dead code.
- They stop after "it should work" instead of verifying a concrete goal.

## The Solution

Four principles in reusable Codex formats:

| Principle | Addresses |
| --- | --- |
| **Think Before Coding** | Wrong assumptions, hidden confusion, missing tradeoffs |
| **Simplicity First** | Overengineering, bloated abstractions, speculative features |
| **Surgical Changes** | Unrelated edits, style drift, unsafe cleanup |
| **Goal-Driven Execution** | Vague tasks, weak success criteria, unverifiable fixes |

This repository includes:

- `AGENTS.md` for project-level Codex instructions.
- `skills/simonstone-codex-guidelines/SKILL.md` for a reusable Codex skill.
- `.codex-plugin/plugin.json` for Codex plugin packaging.
- `EXAMPLES.md` with practical examples of good and bad agent behavior.

## The Four Principles

### 1. Think Before Coding

Do not assume. Do not hide confusion. Surface tradeoffs.

- State assumptions before implementing.
- If multiple interpretations exist, present them instead of silently choosing one.
- If a simpler approach exists, say so.
- If the context is not enough to proceed safely, stop and ask.

### 2. Simplicity First

Write the minimum code that solves the current problem. Nothing speculative.

- No features beyond what was asked.
- No abstractions for single-use logic.
- No optional configuration that was not requested.
- No error handling for scenarios that cannot happen in the current system.
- If the implementation can be much simpler, simplify it first.

The test: would a senior engineer say this solves today's problem, or tomorrow's imagined problem?

### 3. Surgical Changes

Touch only what is required. Clean up only the mess created by the current change.

- Every changed line should trace directly to the user's request.
- Do not refactor, reformat, rename, or "improve" adjacent code.
- Match the existing project style.
- Mention unrelated dead code or tech debt, but do not delete it unless asked.
- Remove imports, variables, functions, or files that your change made unused.

### 4. Goal-Driven Execution

Turn tasks into verifiable goals and loop until the checks are done.

- "Fix the bug" becomes: reproduce it, fix it, verify it no longer happens.
- "Add validation" becomes: cover invalid inputs and confirm valid inputs still pass.
- "Refactor X" becomes: preserve behavior before and after the refactor.
- Multi-step tasks should include a short plan with checks for each step.

## Install

### Option A: Project-level `AGENTS.md`

Copy `AGENTS.md` into the root of a project where you use Codex:

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/simonstone-dev/simonstone-codex-guidelines/main/AGENTS.md
```

If the project already has an `AGENTS.md`, merge the sections instead of replacing project-specific rules.

### Option B: Reusable Codex skill

Copy the skill directory into your Codex skills folder:

```bash
mkdir -p ~/.codex/skills
cp -R skills/simonstone-codex-guidelines ~/.codex/skills/
```

Then ask Codex to use `simonstone-codex-guidelines` for writing, modifying, reviewing, refactoring, or debugging code.

### Option C: Codex plugin packaging

This repository includes `.codex-plugin/plugin.json` and a `skills/` directory so it can be used as a Codex plugin package in environments that support local plugin installation.

## How to Know It's Working

These guidelines are working if you see:

- Fewer unnecessary changes in diffs.
- Simpler first-pass implementations.
- Clarifying questions before implementation, not after mistakes.
- Cleaner pull requests with no drive-by refactoring.
- Final responses that say exactly what was verified.

## Customization

These instructions are designed to be merged with project-specific rules. Add local constraints such as:

```markdown
## Project-Specific Guidelines

- Use TypeScript strict mode.
- Match existing error handling in `src/errors`.
- All API behavior changes need tests.
```

## Tradeoff Note

These guidelines bias toward caution over speed. For trivial tasks, use judgment. The goal is to reduce expensive mistakes on non-trivial work, not to slow down obvious one-line changes.

## Attribution

This project is adapted from the ideas and structure of [`forrestchang/andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills), which was inspired by Andrej Karpathy's observations on common LLM coding-agent pitfalls.

This repository rewrites and packages those ideas for Codex with project-level `AGENTS.md`, a reusable Codex skill, and Codex plugin metadata. It is an independent adaptation and is not affiliated with the original authors.

## License

MIT

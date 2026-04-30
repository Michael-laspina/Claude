---
description: Execute implementation tasks for an approved feature using a TDD red/green/refactor cycle
trigger: user-invoked
---

# Implement Feature

Execute implementation tasks for `$ARGUMENTS` following a strict TDD cycle.

**Prerequisite:** A specification (`.workflow/specs/<slug>.md`) and plan (`.workflow/plans/<slug>.md`) must both exist and be marked as approved.

## Implementation Cycle

For each task in `.workflow/tasks/<slug>.md`:

### 🔴 Red — Write Failing Test
- Write the minimal test that specifies the desired behavior
- Run the test suite and confirm the new test fails
- **Pause** — show the failing test output and ask for confirmation before proceeding

### 🟢 Green — Make It Pass
- Write the minimal production code to make the test pass
- Run the test suite and confirm all tests pass
- No premature optimization or extra logic

### 🔵 Refactor — Clean Up
- Improve code clarity, remove duplication
- Run tests again to confirm nothing broke
- Update inline docs if the behavior is non-obvious

### ✅ Complete — Mark Task Done
- Update `.workflow/tasks/<slug>.md` checkbox
- Commit with message: `feat(<scope>): <task description>`

## Parallel vs Sequential

- **Foundation tasks** (schema, auth, core infrastructure): run sequentially
- **Feature tasks** with no shared state: can be parallelized via git worktrees

## Arguments

`$ARGUMENTS` — required: feature slug matching an existing spec and plan (e.g., `user-authentication`)

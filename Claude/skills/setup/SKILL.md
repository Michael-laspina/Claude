---
description: Initialize a new project with a structured constitution and workflow configuration
trigger: user-invoked
---

# Setup Workflow

Initialize the project's development constitution for structured, spec-first development.

## Steps

1. **Scan the repository** — read existing code, README, and any documentation to understand the project's purpose, tech stack, and conventions.

2. **Generate a constitution** — create `.workflow/constitution.md` containing:
   - Project purpose and scope
   - Technology decisions and constraints
   - Coding standards and conventions
   - Definition of Done for each workflow stage
   - Testing strategy

3. **Scaffold workflow directories**:
   ```
   .workflow/
   ├── constitution.md      ← project standards (created by setup)
   ├── specs/               ← feature specifications
   ├── plans/               ← technical plans
   ├── tasks/               ← implementation task lists
   └── artifacts/           ← generated diagrams / schemas
   ```

4. **Confirm with the user** — present the generated constitution and ask for approval before writing files.

## Arguments

`$ARGUMENTS` — optional: a short description of the project (used to seed the constitution if none exists).

## Output

Print a summary of what was created and instruct the user to run `/claude-workflow:specify <feature>` to begin the first feature workflow.

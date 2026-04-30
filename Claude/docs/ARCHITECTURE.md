# Architecture

## Workflow Stages

```
User Idea
    │
    ▼
/claude-workflow:setup
    │  Creates .workflow/constitution.md
    │  Scaffolds .workflow/ directory
    │
    ▼ (human approves constitution)
    │
/claude-workflow:review <feature>
    │  Requirements Analyst → elicits requirements
    │  Devil's Advocate    → challenges assumptions
    │  Technical Analyst   → assesses feasibility
    │  Saves → .workflow/specs/<slug>.md
    │
    ▼ (human approves spec)
    │
/claude-workflow:plan <feature>          [planned — v1.1]
    │  Technical Analyst → data models, API contracts, tasks
    │  Saves → .workflow/plans/<slug>.md
    │         .workflow/tasks/<slug>.md
    │
    ▼ (human approves plan)
    │
/claude-workflow:implement <feature>
    │  For each task: Red → Green → Refactor
    │  Commits after each green cycle
    │
    ▼
/claude-workflow:audit <feature>
    │  Validates artifact ↔ implementation alignment
    │  Outputs structured audit report
    │
    ▼
  Done ✓
```

## Plugin Components

| Component | Purpose |
|-----------|---------|
| `skills/setup` | Initialize constitution and workflow dirs |
| `skills/review` | Spec generation (3-agent orchestration) |
| `skills/implement` | TDD implementation loop |
| `skills/audit` | Quality gate validation |
| `agents/requirements-analyst` | Elicits structured requirements |
| `agents/devils-advocate` | Challenges spec assumptions |
| `agents/technical-analyst` | Produces technical plan |
| `hooks/hooks.json` | Pre/post tool guards |
| `templates/` | Reusable artifact scaffolds |
| `settings.json` | Default permissions for workflow operations |

## Human Approval Gates

The workflow enforces mandatory human approval at three points:

1. **After setup** — approve the project constitution
2. **After review** — approve the feature specification  
3. **After plan** — approve the technical plan before implementation begins

These gates prevent AI from making architectural decisions without human oversight.

## File Conventions

- Specs: `.workflow/specs/<kebab-slug>.md`
- Plans: `.workflow/plans/<kebab-slug>.md`
- Tasks: `.workflow/tasks/<kebab-slug>.md`
- Constitution: `.workflow/constitution.md`

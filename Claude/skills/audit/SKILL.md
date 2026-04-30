---
description: Audit workflow artifacts and implementation quality for a feature
trigger: user-invoked
---

# Audit Feature

Perform a structured quality audit of all artifacts and code for `$ARGUMENTS`.

## Audit Checklist

### Specification Quality
- [ ] All acceptance criteria are testable
- [ ] Non-functional requirements have measurable thresholds
- [ ] Edge cases and failure modes are documented
- [ ] No ambiguous requirements remain

### Plan Quality
- [ ] All spec requirements are traceable to plan components
- [ ] Data models are complete and consistent
- [ ] API contracts match specification acceptance criteria
- [ ] No orphaned components (every component serves a requirement)

### Task Coverage
- [ ] Every plan component maps to at least one task
- [ ] Tasks are vertically sliced (thin end-to-end slices preferred)
- [ ] Each task has a clear Definition of Done

### Implementation Quality
- [ ] Test coverage meets the project threshold (from constitution)
- [ ] All acceptance criteria have corresponding tests
- [ ] No skipped or pending tests without documented reason
- [ ] Code follows project conventions (from constitution)

## Output Format

```json
{
  "feature": "<slug>",
  "score": "<pass|warn|fail>",
  "checks": [
    { "category": "specification", "status": "pass|warn|fail", "notes": "..." },
    { "category": "plan",          "status": "pass|warn|fail", "notes": "..." },
    { "category": "tasks",         "status": "pass|warn|fail", "notes": "..." },
    { "category": "implementation","status": "pass|warn|fail", "notes": "..." }
  ],
  "summary": "<one paragraph>"
}
```

Print the JSON and a human-readable summary. A `fail` score blocks the workflow until issues are resolved.

## Arguments

`$ARGUMENTS` — required: feature slug to audit (e.g., `user-authentication`)

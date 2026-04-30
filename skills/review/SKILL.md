---
description: Generate a structured specification for a feature or change before any implementation begins
trigger: user-invoked
---

# Specify Feature

Produce a reviewable specification artifact for `$ARGUMENTS` before any code is written.

## Agents Invoked

This skill orchestrates three agents in sequence:

1. **Requirements Analyst** — elicits functional and non-functional requirements
2. **Devil's Advocate** — challenges assumptions and surfaces edge cases
3. **Technical Analyst** — assesses feasibility, risks, and integration points

## Process

### Phase 1 — Requirements (Requirements Analyst)
- Identify actors and user stories
- Define acceptance criteria (Given/When/Then)
- Surface non-functional requirements (performance, security, accessibility)

### Phase 2 — Scrutiny (Devil's Advocate)
- Challenge every assumption in the requirements
- Enumerate failure modes and edge cases
- Flag scope creep or missing context

### Phase 3 — Feasibility (Technical Analyst)
- Assess technical feasibility given the current stack
- Identify external dependencies and integration risks
- Estimate rough complexity (S/M/L/XL)

## Output

Save the specification to `.workflow/specs/<feature-slug>.md` with this structure:

```markdown
# Specification: <Feature Name>

## Summary
<one-paragraph description>

## Requirements
### Functional
- [ ] <requirement>

### Non-Functional
- [ ] <requirement>

## Acceptance Criteria
### Scenario: <name>
- Given <context>
- When <action>
- Then <outcome>

## Risks & Edge Cases
- <risk>

## Feasibility Assessment
- Complexity: <S|M|L|XL>
- Key risks: <list>
- Dependencies: <list>
```

**Human approval required** before proceeding to `/claude-workflow:plan`.

## Arguments

`$ARGUMENTS` — required: feature name or short description (e.g., `user authentication`, `payment integration`)

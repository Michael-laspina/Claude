# claude-workflow

A Claude Code plugin that enforces structured, human-in-the-loop software development. Every feature goes through **Idea → Spec → Plan → Implement** with mandatory human approval gates before any code is written.

Inspired by [human-in-loop](https://github.com/deepeshBodh/human-in-loop).

## Install

```bash
# Inside Claude Code
/plugin marketplace add michael-laspina/claude
/plugin install claude-workflow
```

## Usage

```bash
# 1. Initialize your project (run once)
/claude-workflow:setup

# 2. Specify a feature (produces a reviewable spec)
/claude-workflow:review user authentication

# 3. Implement after spec is approved
/claude-workflow:implement user-authentication

# 4. Audit quality
/claude-workflow:audit user-authentication
```

## Skills

| Command | Description |
|---------|-------------|
| `/claude-workflow:setup` | Initialize project constitution and workflow directories |
| `/claude-workflow:review <feature>` | Generate structured spec via 3-agent review |
| `/claude-workflow:implement <feature>` | TDD implementation loop (red/green/refactor) |
| `/claude-workflow:audit <feature>` | Quality audit of artifacts and implementation |

## Agents

| Agent | Role |
|-------|------|
| `requirements-analyst` | Elicits testable requirements and acceptance criteria |
| `devils-advocate` | Challenges assumptions and surfaces edge cases |
| `technical-analyst` | Assesses feasibility and produces technical plans |

## Workflow Artifacts

All artifacts are saved to `.workflow/` in your project:

```
.workflow/
├── constitution.md      ← project standards and conventions
├── specs/               ← feature specifications (one per feature)
├── plans/               ← technical plans (one per feature)
├── tasks/               ← implementation task checklists
└── artifacts/           ← diagrams, schemas, etc.
```

## Philosophy

- **Humans decide, AI executes** — no architectural decisions without human approval
- **Specs before code** — requirements are locked before implementation begins
- **TDD by default** — every task follows red/green/refactor
- **Auditable artifacts** — every decision is documented and traceable

## Development

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Test plugin locally
claude --plugin-dir ./

# Inside Claude Code
/claude-workflow:setup
```

## License

MIT

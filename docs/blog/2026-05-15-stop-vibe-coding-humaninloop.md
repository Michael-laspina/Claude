# Stop Vibe Coding: How HumanInLoop Brings Discipline Back to AI-Assisted Development

*Published: May 15, 2026*

---

There's a pattern that emerges when developers first start using AI coding assistants: it feels like magic. You describe what you want, the assistant writes it, you ship it. Fast, frictionless, fun.

Then, six weeks later, you're staring at a 3,000-line file no one fully understands, a dependency graph that looks like a plate of spaghetti, and a test suite that covers 40% of the codebase — optimistically. The AI was helpful. But it was helpful the way a genie is helpful: it did exactly what you asked, not what you meant.

This is vibe coding. And it's a silent killer of software quality.

## The Problem With Letting AI Improvise

AI coding assistants are remarkably capable at the tactical level — writing functions, fixing bugs, generating boilerplate. But architecture is a different story. Without explicit structure, an AI will make architectural decisions constantly, implicitly, based on what "feels" right in the context of the last few messages. Each decision seems reasonable in isolation. Accumulated over a project, they create systems that are hard to reason about, impossible to audit, and expensive to change.

The root cause isn't the AI. It's the absence of a forcing function that makes humans do the hard thinking *before* the code is written.

## Specification-Driven Development at the Speed of AI

HumanInLoop is a Claude Code plugin built around a single conviction: **humans must own the architecture, and AI must prove it can execute before writing a line of production code.**

The workflow enforces a four-stage gate:

```
Idea → Specification → Plan → Tasks → Implementation
```

Each stage is a checkpoint where human review happens before the next stage can proceed. The AI generates artifacts — requirements documents, technical specifications, task breakdowns — and humans approve or redirect them. The AI never improvises its way past a gate.

This sounds slow. In practice, it's faster than cleaning up after vibe coding.

## How It Works: DAG-Based Execution

Under the hood, HumanInLoop uses a directed acyclic graph (DAG) to model each workflow stage. The `humaninloop_brain` Python package — built on NetworkX and Pydantic — provides deterministic graph execution. Every node in the graph corresponds to a workflow step. Every edge encodes a dependency.

This matters because determinism is the enemy of surprise. When the workflow completes a node, it validates the output against a schema before marking it done and advancing. When a node fails, the graph can tell you exactly where and why. There's no "the agent decided to skip that step" — the graph won't allow it.

The graph state is persisted as JSON and accessible via a read-only HTTP API (introduced in v3.3.2), which means you can point a web UI at a running workflow and see live kanban-style status without touching the CLI. Observability is first-class, not an afterthought.

## Nine Specialists, Not One Generalist

HumanInLoop routes work to nine specialized agents rather than asking a single generalist to do everything:

| Agent | Role |
|---|---|
| Requirements Analyst | Elicits and structures requirements from ambiguous inputs |
| Technical Analyst | Evaluates feasibility, surfaces technical constraints |
| Devil's Advocate | Stress-tests assumptions, identifies failure modes |
| Principal Architect | Owns the technical design and makes final architecture calls |
| Task Architect | Decomposes approved designs into executable task lists |
| Staff Engineer | Implements code through strict TDD discipline |
| QA Engineer | Validates implementation against acceptance criteria |
| UI Designer | Owns interface and interaction design |
| State Analyst | Manages DAG state, briefs agents, advances workflow |

Each agent has a defined scope, a set of skills it can invoke, and explicit instructions about what it must *not* do. Coupling leaks — where an agent makes assumptions about what another agent did or will do — are treated as bugs, not implementation details.

This separation is deliberate. A requirements analyst shouldn't know which engineer will implement the feature. A staff engineer shouldn't need to understand the DAG topology. Isolation is what makes the system predictable at scale.

## The MCP Transport Layer

In v3.3.0, HumanInLoop migrated its core DAG operations from shell scripts to a FastMCP server. The `hil-dag` binary now runs as a Model Context Protocol server over stdio, exposing seven tools: `assemble`, `validate`, `sort`, `status`, `record`, `freeze`, and `catalog_validate`.

Why does this matter? Because MCP calls return structured JSON with consistent `checks` and `summary` fields. Every tool invocation is machine-readable, auditable, and composable. Agents that previously had to parse shell output now receive typed responses. The business logic lives in `mcp/operations.py` and is shared by both the MCP server and a thin CLI adapter — transport is decoupled from behavior.

The practical effect: fewer surprises, better error messages, and a foundation for the team collaboration and compliance auditing features on the roadmap.

## What "Stop Vibe Coding" Actually Means

It doesn't mean distrust AI. It means respecting that AI is a powerful executor, not a reliable architect.

When you run `/humaninloop:specify`, you're not asking the AI to build something. You're asking the Requirements Analyst to interview you, the Technical Analyst to assess what you described, and the Devil's Advocate to tell you what could go wrong — all before the Principal Architect drafts a design. That design goes to you for review. Only after you approve it does the Task Architect break it into work.

By the time a Staff Engineer writes code, the architecture has been reviewed by a human. The TDD cycles are verified by the QA Engineer. The final validation gate checks the implementation against the original requirements.

This is specification-driven development. The AI does more work, not less — but the work is structured, reviewable, and reversible at every stage.

## Getting Started

HumanInLoop is available as a Claude Code plugin. Add it to your Claude Code configuration and run `/humaninloop:setup` to initialize a project.

```
/humaninloop:setup    # Initialize project structure
/humaninloop:specify  # Start a new feature specification
/humaninloop:plan     # Generate implementation plan from approved spec
/humaninloop:tasks    # Break plan into executable task list
/humaninloop:implement # Execute tasks with TDD discipline
/humaninloop:audit    # Review workflow artifacts and coverage
```

The full changelog, roadmap, and architecture documentation live in the repository. The `humaninloop_brain` package is open source and independently testable — 418 tests, ~95% coverage.

---

Software that lasts is software that was designed. HumanInLoop makes sure the design happens before the code does.

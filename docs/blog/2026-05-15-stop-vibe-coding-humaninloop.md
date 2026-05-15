# Stop Vibe Coding: How HumanInLoop Puts Humans Back in the Driver's Seat

**Published:** 2026-05-15  
**Tags:** AI, Claude Code, specification-driven development, software engineering

---

AI coding assistants are extraordinary. They can generate a working REST API, scaffold a React app, and write database migrations in seconds. The problem is that they'll do it whether or not you've thought clearly about what you actually need.

"Vibe coding" — asking an AI to build something based on a rough feeling rather than a precise specification — is the new tech debt. It's fast to start and expensive to finish. The code works, more or less, but it doesn't match what you meant. Edge cases weren't considered. The data model doesn't quite fit the domain. The architecture painted you into a corner. And now you own it.

HumanInLoop exists to stop this pattern before it starts.

---

## What HumanInLoop Does

HumanInLoop is a Claude Code plugin that enforces **specification-driven development** as a workflow. It doesn't prevent you from using AI to write code — it ensures that before any code is written, a human has made the key decisions that determine whether the code will actually be right.

The plugin installs as a set of commands that guide you through a structured lifecycle:

```
/humaninloop:setup      → Establish your project's governance constitution
/humaninloop:specify    → Define what you're building (requirements + adversarial review)
/humaninloop:plan       → Produce the technical blueprint
/humaninloop:tasks      → Break the plan into vertical TDD cycles
/humaninloop:implement  → Execute with test-driven discipline and checkpoints
/humaninloop:audit      → Validate artifacts for quality and alignment
```

Each command is a gate. You don't get to `/plan` without a specification. You don't get to `/implement` without a plan. The workflow is enforced, not suggested.

---

## The Specification Phase: Two Agents, One Goal

The heart of the system is `/humaninloop:specify`. When you invoke it, two agents are activated:

- **The Requirements Analyst** works with you to capture what you want: the user story, acceptance criteria, constraints, non-functional requirements.
- **The Devil's Advocate** challenges the draft specification — probing for ambiguity, missing edge cases, and unstated assumptions.

The output is a committed specification file in `specs/in-progress/`. It's not a chat transcript. It's a structured artifact that the rest of the workflow depends on.

This adversarial review step is the highest-leverage part of the system. The bugs caught here cost nothing to fix. The bugs caught in production are the expensive ones.

---

## The Planning Phase: Decisions Before Code

`/humaninloop:plan` takes your specification and produces a technical blueprint: data models, API contracts, architectural constraints, and dependency decisions. A **Principal Architect** agent drives this phase, with a **Technical Analyst** providing domain analysis.

The plan is also committed to the repository. It becomes the ground truth for the implementation phase. If you discover during implementation that the plan is wrong, you go back and amend the plan — you don't just change the code and leave the plan stale.

---

## The Infrastructure: Deterministic DAGs

What makes HumanInLoop more than a set of clever prompts is its infrastructure layer: the `humaninloop_brain` Python package.

Every workflow execution is represented as a **directed acyclic graph** (DAG). Each node in the graph corresponds to a task — a discrete unit of work that an agent will perform. The DAG is:

- **Assembled** from a catalog of capabilities
- **Validated** for structural correctness before execution begins
- **Sorted** topologically to enforce dependency ordering
- **Frozen** once execution starts, preventing mid-flight modifications

The graph execution is deterministic. Given the same inputs and the same specification, the same graph is produced. This is by design. Agents operate within the constraints the graph defines — they don't improvise the structure of work, only the content of it.

The `hil-dag` MCP server is the sole write gate for StrategyGraph JSON. Agents make tool calls against it. They don't write JSON directly. This keeps the infrastructure layer clean and auditable.

```python
# The brain enforces what the agents consume
# Tier 1: strict graph-algorithmic (NetworkX)
# Tier 2: heuristic-deterministic (catalog-driven assembly)
# Agents consume via CLI / MCP tool calls — never write JSON directly
```

As of v3.3.2, the DAG state is also exposed via a read-only HTTP API on port 8100, enabling real-time kanban visualization of workflow progress.

---

## Nine Agents, One Workflow

HumanInLoop's 9 specialized agents each own a narrow domain:

| Agent | Role |
|---|---|
| Requirements Analyst | Captures and structures requirements |
| Devil's Advocate | Challenges specifications for gaps and ambiguity |
| Principal Architect | Produces technical blueprints |
| Technical Analyst | Domain analysis and constraint identification |
| Task Architect | Decomposes plans into vertical TDD cycles |
| State Analyst | Manages DAG assembly, node status, and workflow progression |
| QA Engineer | Test strategy and acceptance criteria |
| UI Designer | Interface and interaction design |
| Supervisor | Orchestrates agent dispatch and workflow flow |

The State Analyst is particularly interesting. In v3.3.1, it absorbed the DAG Assembler's responsibilities, reducing per-node execution from 6 Supervisor round-trips to 2. This wasn't a cosmetic change — it reflected a genuine architectural insight: node assembly and state analysis are the same concern, and splitting them across agents added latency without adding clarity.

---

## The Quality Bar

The plugin is built with the same rigor it enforces on your projects:

- **418 tests**, ~95% coverage on `humaninloop_brain`
- **90% coverage is a blocking CI gate** — PRs that drop below it don't merge
- **Conventional Commits** enforced by pre-commit hook and CI
- **Pydantic entity models** with frozen instances and type-status validation
- **8 Architecture Decision Records** documenting every significant design choice

The constitution governing the project is checked into the repository at `.humaninloop/memory/constitution.md`. Every principle in it is enforced by CI, not by convention.

---

## Why This Matters Now

AI coding assistants are going to keep getting better. The gap between "what you asked for" and "what you got" will narrow at the code level. But the gap between "what you asked for" and "what you needed" is a human problem. It's not solvable by a better model.

The discipline of specification-driven development predates AI coding by decades. What HumanInLoop does is make that discipline the path of least resistance when you're working with AI. The workflow isn't a bureaucratic tax on your velocity — it's the thing that makes your velocity sustainable.

Vibe coding is fast until it isn't. Specification-driven development is slower to start and faster to finish.

---

## Getting Started

HumanInLoop is available as a Claude Code plugin. To install:

1. Add the plugin to your Claude Code configuration
2. Run `/humaninloop:setup` to establish your project's constitution
3. Start your next feature with `/humaninloop:specify`

The source code, documentation, and changelog are in this repository. The constitution is at `.humaninloop/memory/constitution.md`. Read it — it explains not just what the system does, but why every decision was made the way it was.

---

*HumanInLoop is an open-source Claude Code plugin. Contributions follow the spec-driven development workflow described above — new features begin with an issue and a specification, not a PR.*

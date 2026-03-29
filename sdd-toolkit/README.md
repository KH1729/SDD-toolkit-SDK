# SDD Toolkit

A lean, human-governed, token-aware, multi-agent Spec-Driven Development system.

## What Is This?

SDD Toolkit enforces a disciplined development flow where every feature moves through a structured pipeline:

**Idea → Spec → Design → Plan → Tasks → Implementation → Validation**

Each phase produces a draft artifact, a summary, and a handoff. Only after explicit human approval is the artifact versioned in the registry and the toolkit allowed to move to the next phase.

## Core Principles

1. **Human approval by default.** No phase transition occurs without explicit developer sign-off. Agents draft, recommend, and validate — they do not self-approve.
2. **Disciplined flow.** The toolkit follows a disciplined default pipeline. Phases are not skipped, reordered, or advanced unless the developer explicitly approves or instructs it.
3. **Token-aware execution.** Summaries first, scoped context loading, structured handoffs, diff-based validation, and dynamic scaling only when beneficial.
4. **Versioned artifacts.** Only approved artifacts are snapshotted in the registry with a version number. Drafts remain in the workspace and are never treated as canonical.
5. **Shared execution memory.** Agents read from a common memory area — phase state, summaries, handoffs, and validation findings — instead of re-reading raw artifacts.

## Agent Model

### Fixed Agents

| Agent | Role |
|---|---|
| **Master Agent** | Orchestrator, resource planner, gate enforcer |
| **Spec Agent** | Turns idea into a bounded, testable spec |
| **Architecture Agent** | Produces lean technical design from approved spec |
| **Work Manager Agent** | Creates plan, task breakdown, and worker allocation |

### Elastic Agents

| Agent | Role |
|---|---|
| **Worker Agent(s)** | Implement assigned tasks within scoped boundaries |
| **Validation Agent(s)** | Review artifacts and implementation against spec and rules |

The toolkit activates only the minimum set of roles needed for the current phase. Worker and validation capacity may scale based on complexity, token budget, and model capability, but role consolidation is preferred when it keeps the system lean.

## Folder Structure

```
sdd-toolkit/
├── commands/        # Executable command definitions
├── agents/          # Agent role definitions and starter prompts
├── rules/           # Governance and engineering rules
├── templates/       # Reusable artifact templates
├── orchestrator/    # Workflow, state machine, resource allocation
├── memory/          # Shared execution memory (state, summaries, handoffs)
├── registry/        # Versioned approved artifacts
└── workspace/       # Active working area per feature
```

### Key Directories

- **memory/** — Agents consult shared memory first — phase state, summaries, handoffs, prior decisions, and validation findings — and only load raw artifacts or code when needed and within scope.
- **registry/** — Immutable versioned snapshots of approved artifacts. Once a phase is approved, its artifact is stored here (e.g., `registry/specs/my-feature/spec/v1.md`).
- **workspace/** — Active working area. Contains the current draft of each artifact and implementation outputs (task board, worker assignments, code).

## Main Commands

| Command | Purpose |
|---|---|
| `build-feature` | Run the full SDD pipeline for a single feature |
| `build-project` | Run the pipeline for an entire project (multiple features) |
| `approve-phase` | Approve the current phase and advance the workflow |
| `request-rework` | Reject the current phase and request changes |
| `validate-feature` | Run validation against a feature's artifacts |

## Default Mode

The toolkit operates in **manual** mode by default. Every phase transition requires explicit human approval. To enable more autonomous execution, change the mode in `toolkit-config.yaml`.

## Getting Started

1. Copy or fork this toolkit into your project.
2. Review `toolkit-config.yaml` and adjust settings if needed.
3. Use `build-feature` to start the pipeline for a new feature.
4. Review and approve each phase as the agents produce artifacts.
5. Approved artifacts are versioned in the registry automatically.

## Skills

The toolkit exposes its core workflow operations as skills.

Available skills:

- `build-feature`
- `build-project`
- `request-rework`
- `validate-feature`

Each skill is defined under:

```text
skills/<skill-name>/SKILL.md
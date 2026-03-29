# SDD Toolkit

A lean, human-governed, token-aware, multi-agent Spec-Driven Development system.

## What Is This?

SDD Toolkit enforces a disciplined development flow where every feature moves through a structured pipeline:

**Idea → Spec → Design → Plan → Tasks → Implementation → Validation**

Each phase produces a draft artifact, a summary, and a handoff. Only after explicit human approval is the artifact versioned in the registry and the toolkit allowed to move to the next phase.

## Quick Start

Run the initializer to set up any project with SDD Toolkit:

```bash
# From this repo — initialize a target project
./init.sh /path/to/your/project

# Or initialize the current directory
./init.sh
```

This creates:

- `.cursor/rules/` — 11 Cursor rule files that give the AI agent full knowledge of the SDD workflow, agent roles, governance, and commands
- `sdd/` — Runtime directories (templates, memory, workspace, registry)
- `toolkit-config.yaml` — Central configuration

After initialization, open your project in Cursor and start working:

- **"Build feature: user-notifications — Add real-time notification system with email and push support"**
- **"Build project: crm-platform — Build a CRM with contacts, deals, and reporting"**
- **"Approve phase"** / **"Request rework: the spec is missing error handling for edge cases"**

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

## Commands

| Command | Purpose |
|---|---|
| `build-feature` | Run the full SDD pipeline for a single feature |
| `build-project` | Run the pipeline for an entire project (multiple features) |
| `approve-phase` | Approve the current phase and advance the workflow |
| `request-rework` | Reject the current phase and request changes |
| `validate-feature` | Run validation against a feature's artifacts |

## What the Initializer Creates

### Cursor Rules (`.cursor/rules/`)

| Rule | Type | Purpose |
|---|---|---|
| `sdd-overview.mdc` | Always active | Compact workflow overview, folder layout, command reference |
| `sdd-master-agent.mdc` | Agent-requested | Master Agent role, state machine, workflow definition |
| `sdd-spec-agent.mdc` | Agent-requested | Spec Agent role and starter prompt |
| `sdd-architecture-agent.mdc` | Agent-requested | Architecture Agent role and starter prompt |
| `sdd-work-manager-agent.mdc` | Agent-requested | Work Manager role and starter prompt |
| `sdd-worker-agent.mdc` | Agent-requested | Worker Agent role and starter prompt |
| `sdd-validation-agent.mdc` | Agent-requested | Validation Agent role and validation rules |
| `sdd-governance.mdc` | Agent-requested | Workflow, approval, engineering, architecture rules |
| `sdd-token-efficiency.mdc` | Agent-requested | Token optimization and resource allocation |
| `sdd-escalation.mdc` | Agent-requested | Escalation policy and triggers |
| `sdd-commands.mdc` | Agent-requested | Command definitions with routing hints |

Only the overview rule is always loaded. All other rules are pulled in by the AI agent on demand based on the current task, keeping token usage low.

### Runtime Directories (`sdd/`)

```
sdd/
  templates/       — Reusable artifact templates (idea, spec, design, plan, tasks, validation, handoff, summary)
  memory/          — Shared execution memory
    feature-state/ — Per-feature state JSON files
    summaries/     — Phase summaries
    handoffs/      — Agent-to-agent handoff documents
    decisions/     — Decision log
  workspace/       — Active working area per feature
  registry/        — Versioned approved artifacts
```

### Configuration (`toolkit-config.yaml`)

Controls execution mode (manual/guided/delegated), phase flow, token budgets, worker/validator scaling, and artifact paths.

## Default Mode

The toolkit operates in **manual** mode by default. Every phase transition requires explicit human approval. To enable more autonomous execution, change the mode in `toolkit-config.yaml`.

## Customization

After running `init.sh`, you can customize:

- **`toolkit-config.yaml`** — Change the default mode, adjust worker/validator limits, set token budgets
- **`.cursor/rules/*.mdc`** — Edit any rule to adjust agent behavior, add project-specific constraints, or change governance
- **`sdd/templates/`** — Modify artifact templates to fit your project's conventions

## Repository Structure

This repository is the **source** for the SDD Toolkit. The `init.sh` script is the primary output — it embeds all rules, templates, and configuration into a single portable initializer.

```
SDD-toolkit-SDK/
  init.sh              — The initializer script (run this)
  README.md            — This file
  agents/              — Source: agent role definitions
  commands/            — Source: command specifications
  rules/              — Source: governance rules
  orchestrator/        — Source: workflow, state machine, escalation
  skills/              — Source: skill definitions
  templates/           — Source: artifact templates
  toolkit-config.yaml  — Source: default configuration
```

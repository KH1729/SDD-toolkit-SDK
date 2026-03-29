> This command is implemented as the `build-project` skill in `skills/build-project/SKILL.md`.
> This file remains the human-readable command reference.
# Command: build-project

## Purpose

Run the SDD pipeline for an entire project by decomposing it into features and running `build-feature` for each.

## Expected Input

- **Project name** — a short identifier (e.g., `user-management-platform`)
- **Project description** — high-level description of the project
- **Options** (optional):
  - `--mode manual|semi-auto|auto` — override default execution mode
  - `--token-budget <number>` — total token ceiling across all features
  - `--parallel` — allow parallel feature pipelines (only in semi-auto or auto mode)

## Example Usage

```
build-project user-management-platform "Build a user management platform with registration, bulk import, role management, and audit logging"
```

## Behavior

1. **Project Capture** — Master Agent captures the project description and decomposes it into discrete features.
2. **Feature List Review** — The proposed feature list is presented for human approval. The developer may add, remove, reorder, or merge features.
3. **Sequential Execution** — Each approved feature runs through `build-feature` in the agreed order. In manual mode, each feature completes fully (including all approval gates) before the next begins.
4. **Cross-Feature Coordination** — The Master Agent maintains a project-level view, tracking shared dependencies, architecture decisions, and interface contracts across features.
5. **Project Validation** — After all features complete, a final project-level validation pass checks cross-feature integration, consistency, and completeness.

## Outputs

- All individual feature outputs (workspace, memory, registry)
- Project-level summary in `memory/summaries/<project>/project-summary.md`
- Project feature list and dependency map

## Important Rules

- Feature decomposition must be approved by the human developer before execution begins.
- Features run independently through the pipeline, each with their own approval gates.
- Cross-feature dependencies must be identified during the plan phase and tracked by the Master Agent.
- In manual mode, parallel execution is not permitted — features run sequentially.

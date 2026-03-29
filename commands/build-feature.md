> This command is implemented as the `build-feature` skill in `skills/build-feature/SKILL.md`.
> This file remains the human-readable command reference.
# Command: build-feature

## Purpose

Run the full SDD pipeline for a single feature, moving it through every phase from idea capture to validation.

## Expected Input

- **Feature name** — a short identifier (e.g., `bulk-user-import`)
- **Raw idea** — free-text description of what the feature should do
- **Options** (optional):
  - `--mode manual|semi-auto|auto` — override default execution mode
  - `--token-budget <number>` — set a token ceiling for this run
  - `--workers <number>` — override default worker count

## Example Usage

```
build-feature bulk-user-import "Add CSV-based bulk user import with validation, error reporting, and rollback"
```

## Behavior

1. **Idea Capture** — Master Agent captures the raw idea, normalizes it using the idea template, and writes it to `workspace/<feature>/current/idea.md`.
2. **Pause for Approval** — In manual mode, the workflow pauses. The developer reviews and approves or requests rework.
3. **Spec Drafting** — On approval, Spec Agent reads the idea summary and produces a spec using the spec template. Writes to `workspace/<feature>/current/spec.md`. Emits summary and handoff.
4. **Pause for Approval** — Developer reviews spec.
5. **Design** — Architecture Agent reads the approved spec summary and handoff, produces a design. Writes to `workspace/<feature>/current/design.md`. Emits summary and handoff.
6. **Pause for Approval** — Developer reviews design.
7. **Plan + Tasks** — Work Manager Agent reads approved design, produces plan and tasks. Writes to `workspace/<feature>/current/plan.md` and `tasks.md`. Proposes worker/validator allocation.
8. **Pause for Approval** — Developer reviews plan and tasks.
9. **Implementation** — Worker Agents execute assigned tasks. Outputs go to `workspace/<feature>/implementation/outputs/`.
10. **Pause for Approval** — Developer reviews implementation.
11. **Validation** — Validation Agents check implementation against design, spec, and rules. Write findings to `workspace/<feature>/current/validation.md`.
12. **Final Decision** — Developer reviews validation results and approves, requests rework, or closes the feature.

At each approval step, the approved artifact is versioned in the registry.

## Outputs

- All phase artifacts in `workspace/<feature>/current/`
- Implementation outputs in `workspace/<feature>/implementation/`
- Feature state in `memory/feature-state/<feature>.json`
- Phase summaries in `memory/summaries/<feature>/`
- Handoffs in `memory/handoffs/<feature>/`
- Versioned snapshots in `registry/specs/<feature>/`

## Important Rules

- In manual mode, **every** phase gate requires explicit human approval.
- No phase may be skipped or reordered.
- If any phase fails validation or is rejected, the workflow routes to rework — it does not silently proceed.
- The Master Agent controls resource allocation and may adjust worker/validator counts between phases.

# Command: approve-phase

## Purpose

Approve the current phase of a feature's pipeline and advance the workflow to the next phase.

## Expected Input

- **Feature name** — the feature to approve
- **Phase** (optional) — explicit phase name; defaults to current pending phase
- **Options** (optional):
  - `--notes "..."` — attach approval notes
  - `--with-modifications` — approve but flag that minor modifications were made to the artifact before approval

## Example Usage

```
approve-phase bulk-user-import
approve-phase bulk-user-import --phase spec --notes "Approved with scope narrowed to CSV only"
```

## Behavior

1. **Validation** — Confirms the feature is in a `WAITING_FOR_HUMAN_*_APPROVAL` state. Rejects if the feature is not at an approval gate.
2. **Snapshot** — Takes the current artifact from `workspace/<feature>/current/` and versions it in the registry (e.g., `registry/specs/<feature>/spec/v1.md`). If a version already exists, increments the version number.
3. **State Update** — Updates `memory/feature-state/<feature>.json` to mark the phase as approved and advance `current_phase`.
4. **Transition** — Triggers the next phase in the pipeline. The appropriate agent begins work.

## Outputs

- Updated feature state in memory
- Versioned artifact snapshot in registry
- Workflow advances to next phase

## Important Rules

- Only valid at an approval gate. If the feature is mid-phase or not at a gate, the command fails with a clear message.
- Approval is irreversible for that version — but the artifact can be revised in a later rework cycle, producing a new version.
- The developer may attach notes that are recorded in the feature state and visible to downstream agents.

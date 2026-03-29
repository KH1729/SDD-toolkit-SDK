> This command is implemented as the `request-rework` skill in `skills/request-rework/SKILL.md`.
> This file remains the human-readable command reference.
# Command: request-rework

## Purpose

Reject the current phase output and send it back for revision with specific feedback.

## Expected Input

- **Feature name** — the feature requiring rework
- **Feedback** — what needs to change
- **Options** (optional):
  - `--phase <name>` — target a specific phase (defaults to current)
  - `--severity low|medium|high` — urgency of rework
  - `--rollback-to <phase>` — roll back to an earlier phase if the issue is fundamental

## Example Usage

```
request-rework bulk-user-import "Spec is missing error handling requirements for malformed CSV rows"
request-rework bulk-user-import --rollback-to idea "Scope needs to be fundamentally reconsidered"
```

## Behavior

1. **Validation** — Confirms the feature is at an approval gate or in a reviewable state.
2. **Record Feedback** — Stores the rework request in `memory/feature-state/<feature>.json` including the feedback text, severity, and target phase.
3. **State Rollback** — Moves the feature state back to the target phase's drafting state (e.g., `SPEC_DRAFTED` if spec needs rework). If `--rollback-to` is specified, rolls back further.
4. **Notify Agent** — The responsible agent is re-invoked with the rework feedback and the existing artifact as context.

## Outputs

- Updated feature state reflecting the rework request
- Rework feedback recorded in memory
- Responsible agent re-engaged with feedback

## Important Rules

- Rework feedback must be specific and actionable. Vague rejections waste tokens.
- Rollback to an earlier phase invalidates all downstream artifacts — they will need to be re-drafted after the earlier phase is re-approved.
- Rework cycles are tracked in the feature state. Excessive rework on a single phase triggers an escalation recommendation.

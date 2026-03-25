# Command: validate-feature

## Purpose

Run validation checks against a feature's artifacts and implementation at any point in the pipeline.

## Expected Input

- **Feature name** — the feature to validate
- **Options** (optional):
  - `--scope full|current-phase|implementation` — what to validate (default: `current-phase`)
  - `--validators <number>` — override validator count
  - `--strict` — treat warnings as failures

## Example Usage

```
validate-feature bulk-user-import
validate-feature bulk-user-import --scope full --strict
```

## Behavior

1. **Scope Resolution** — Determines what to validate based on the `--scope` flag:
   - `current-phase` — validates only the current phase artifact against the previous phase
   - `full` — validates all completed artifacts for internal consistency and spec compliance
   - `implementation` — validates code and outputs against the approved design and task definitions
2. **Validation Execution** — Validation Agents run checks and produce findings.
3. **Report** — Writes validation results to `workspace/<feature>/current/validation.md` using the validation template.
4. **State Update** — Updates feature state with validation status and findings summary.

## Outputs

- Validation report in `workspace/<feature>/current/validation.md`
- Summary in `memory/summaries/<feature>/validation-summary.md`
- Updated feature state

## Important Rules

- Validation agents review and report — they do not fix issues.
- Each finding must have a severity level (info, warning, error, critical).
- A single critical finding or any error in `--strict` mode results in an overall FAIL.
- Validation results are presented to the human developer for decision — the system does not auto-reject or auto-approve based on validation.

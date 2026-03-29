# Plan: Bulk User Import

## Execution Strategy

- Approach: sequential phases, parallel tasks within Phase 2 where dependencies allow
- Estimated workers: 2
- Estimated validators: 1

## Work Phases

### Phase 1: Database Foundation
- **Goal:** Add the `import_batch_id` column to the users table.
- **Tasks:** T-001
- **Dependencies:** None
- **Validation checkpoint:** No

### Phase 2: Core Import Logic
- **Goal:** Implement CSV parsing, validation pipeline, and import executor.
- **Tasks:** T-002, T-003, T-004, T-005
- **Dependencies:** Phase 1 complete
- **Validation checkpoint:** Yes

### Phase 3: API Integration
- **Goal:** Wire up the upload endpoint and error report generation.
- **Tasks:** T-006
- **Dependencies:** Phase 2 complete
- **Validation checkpoint:** Yes (final)

## Dependency Map

| Task/Phase | Depends On | Blocking? |
|---|---|---|
| T-001 (migration) | Nothing | Yes |
| T-002 (parser) | T-001 | Yes |
| T-003 (structural validator) | T-002 | Yes |
| T-004 (business validator) | T-002 | Yes |
| T-005 (import executor) | T-003, T-004 | Yes |
| T-006 (endpoint + error report) | T-005 | No (final) |

## Integration Approach

Incremental. Each phase builds on the previous. Components are tested independently before integration.

## Rollout Order

1. Database migration
2. Application code with new endpoint
3. Error report cleanup cron configuration

## Risks / Coordination Notes

- Worker 2 has more work during Phase 2. Load rebalancing may be needed.
- Phase 2 validation checkpoint catches core logic issues early.

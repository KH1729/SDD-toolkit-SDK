# Design: Bulk User Import

## Design Summary

The bulk import feature adds a new API endpoint (`POST /api/users/import`) that accepts multipart CSV uploads. The file is processed through a streaming CSV parser, then a two-phase validation pipeline (structural → business). On full validation pass, users are batch-inserted within a database transaction. On failure, a CSV error report is generated and stored temporarily for download.

## Architecture Impact

- Impact level: low
- New components: CSV parser module, validation pipeline, import executor, error report generator
- Modified components: users API router (new endpoint), users table (new column)

## Components / Modules Affected

| Component | Change Type | Description |
|---|---|---|
| `api/routes/users.js` | Modified | Add `POST /import` route |
| `services/import/csv-parser.js` | New | Streaming CSV parsing with header validation |
| `services/import/structural-validator.js` | New | Validates CSV structure, headers, required fields |
| `services/import/business-validator.js` | New | Validates business rules (duplicates, data types) |
| `services/import/import-executor.js` | New | Transactional batch insert with rollback |
| `services/import/error-report.js` | New | Generates CSV error report, manages temp storage |
| `migrations/add-import-batch-id.js` | New | Adds `import_batch_id` column to users table |

## Interfaces / APIs

### POST /api/users/import
- **Input:** Multipart form data with a single `file` field (CSV)
- **Output (success):** `{ "status": "ok", "imported_count": 150, "batch_id": "uuid" }`
- **Output (validation failure):** `{ "status": "error", "error_count": 5, "error_report_url": "/api/users/import/reports/{id}" }`
- **Output (file too large):** `{ "status": "error", "message": "File exceeds maximum size of 10MB" }`
- **Errors:** 400 (validation), 413 (file too large), 401 (unauthorized), 403 (not admin), 500 (server error)

### GET /api/users/import/reports/{id}
- **Input:** Report ID in URL path
- **Output:** CSV file download
- **Errors:** 404 (not found or expired), 403 (not the uploader)

## Data Flow

1. Client uploads CSV file to `POST /api/users/import`.
2. Endpoint checks auth (admin role required) and file size (≤ 10MB).
3. CSV parser streams the file, extracts headers and rows.
4. Structural validator checks headers and row format.
5. If structural validation fails → generate error report → return error response.
6. Business validator checks each row (duplicates, data types, constraints).
7. If business validation fails → generate error report → return error response.
8. Import executor opens database transaction, batch-inserts users (100 per batch), assigns `import_batch_id`.
9. On success → commit transaction → return success response.
10. On failure → rollback transaction → return error response.

## Data Model / Schema Changes

| Entity | Change | Fields |
|---|---|---|
| `users` | Modified | Add `import_batch_id` (UUID, nullable, indexed) |

## Security Considerations

- Endpoint requires admin role authentication.
- Uploaded files are processed in-memory (streaming) and never written to permanent storage.
- Error reports are scoped to the uploading user.
- File size is checked before parsing to prevent resource exhaustion.

## Observability Considerations

- Log import start, row count, validation result, and completion status.
- Log errors with batch ID for traceability.

## Testing Strategy

- Unit tests: CSV parser, structural validator, business validator, import executor, error report generator.
- Integration tests: Full upload → validate → import flow. Full upload → validate → error report flow.
- Edge case tests: Empty file, headers only, 10MB file, malformed CSV, duplicate emails.

## Rollout / Migration Notes

- Database migration must run before deployment.
- Migration is backwards-compatible.
- Error report cleanup cron must be configured in deployment.

## Risks / Tradeoffs

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Timeout on large files (near 10MB) | Medium | Medium | Set 120s timeout, document limitation |
| Memory pressure from streaming large files | Low | Medium | Streaming parser limits memory footprint |
| Orphaned temp error report files | Low | Low | 24h cleanup cron |

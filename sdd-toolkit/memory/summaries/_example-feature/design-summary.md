# Summary: Design — bulk-user-import

## Phase

Design

## What Was Produced

A technical design covering the upload endpoint, CSV parser, two-phase validator, import executor with transaction management, and error report generator.

## Key Decisions

1. New `POST /api/users/import` endpoint accepts multipart file upload.
2. CSV parsing uses the existing `csv-parse` library — no new dependencies needed.
3. Validation is implemented as a pipeline: `StructuralValidator → BusinessValidator`.
4. Import executes within a database transaction; rollback on any critical validation failure.
5. Error report is generated as a CSV file and stored temporarily (24h TTL) with a download URL returned in the response.
6. No new database tables — uses existing `users` table with a new `import_batch_id` column for traceability.

## Risks / Open Issues

- Synchronous processing for large files (near 10MB) may cause request timeouts. Mitigation: set a generous timeout (120s) and document the limitation.
- Adding `import_batch_id` to the users table requires a migration.

## What the Next Agent Must Know

- Five components to implement: endpoint, parser, validator pipeline, import executor, error report generator.
- One database migration needed: add `import_batch_id` column to `users` table.
- Transaction boundary is around the entire import operation.
- Error report is a temporary file with a 24h TTL.

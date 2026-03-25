# Tasks: Bulk User Import

## Task List

### T-001: Database Migration
- **Scope:** Create a migration that adds the `import_batch_id` column (UUID, nullable, indexed) to the `users` table.
- **Inputs:** Design document (data model section)
- **Dependencies:** None
- **Expected Output:** Migration file (forward + rollback)
- **Definition of Done:** Migration runs, column exists, index created, rollback works.
- **Validation Notes:** Check backwards compatibility, nullable, index exists.

### T-002: CSV Parser
- **Scope:** Streaming CSV parser that validates headers and extracts rows as structured objects.
- **Inputs:** Design document (CSV parser component), column schema
- **Dependencies:** T-001
- **Expected Output:** Parser module returning parsed rows or structural error.
- **Definition of Done:** Handles valid files, rejects invalid, streams efficiently. Tests pass.
- **Validation Notes:** Test with empty files, missing headers, malformed CSV.

### T-003: Structural Validator
- **Scope:** Validates CSV structure: required headers, non-empty required fields, correct row format.
- **Inputs:** Parsed CSV data from T-002
- **Dependencies:** T-002
- **Expected Output:** List of structural errors or empty list.
- **Definition of Done:** Catches all structural edge cases. Row-level errors. Tests pass.
- **Validation Notes:** Test every spec edge case.

### T-004: Business Validator
- **Scope:** Validates business rules: duplicate emails (in-file and DB), data types, field constraints.
- **Inputs:** Parsed CSV data from T-002, database access
- **Dependencies:** T-002
- **Expected Output:** List of business rule errors or empty list.
- **Definition of Done:** Detects all duplicate scenarios. Validates types and constraints. Tests pass.
- **Validation Notes:** Test duplicates (in-file, against DB), type validation, constraints.

### T-005: Import Executor
- **Scope:** Transactional batch insert (100/batch) with `import_batch_id` assignment, commit/rollback.
- **Inputs:** Validated rows, database connection
- **Dependencies:** T-003, T-004
- **Expected Output:** Success with count or failure with rollback confirmation.
- **Definition of Done:** Full transaction support. Batch insert works. `import_batch_id` assigned. Tests pass.
- **Validation Notes:** Test success, rollback, batch insert, batch ID assignment.

### T-006: Upload Endpoint and Error Report Generator
- **Scope:** `POST /api/users/import` with multipart handling, auth, file size check. Error report CSV generator with temp storage and download URL.
- **Inputs:** All core components (T-002–T-005), auth middleware
- **Dependencies:** T-005
- **Expected Output:** Working endpoint for full upload-validate-import flow. Downloadable error reports.
- **Definition of Done:** Success and failure flows work. Auth and file size checks work. Error report generated and downloadable. Tests pass.
- **Validation Notes:** End-to-end flow, auth, file size limits, report download, report expiry.

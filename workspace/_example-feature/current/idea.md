# Idea: Bulk User Import

## Raw Request

"We need a way to import users in bulk from a CSV file. It should validate the data, report errors clearly, and roll back if something goes wrong. Start with CSV only."

## Normalized Summary

Add a bulk user import feature that accepts CSV file uploads, validates the data in two phases (structural and business rules), imports valid records transactionally, and produces a downloadable error report for any validation failures.

## Initial Scope

- Accept CSV file upload via API endpoint
- Validate CSV structure (headers, format, required fields)
- Validate business rules (duplicate emails, data types, field constraints)
- Import users transactionally (all-or-nothing)
- Generate downloadable CSV error report with row-level detail
- Support files up to 10MB (~50,000 rows)

## Out of Scope

- Excel, JSON, or other file formats
- Async/background processing
- Real-time progress tracking
- Scheduled or recurring imports
- Partial import (import valid rows, skip invalid ones)

## Assumptions

- The existing `users` table schema is sufficient for imported data
- Email address is the unique identifier for duplicate detection
- The `csv-parse` library is already available in the project

## Constraints

- Synchronous processing only in this iteration
- Must use existing database infrastructure
- No new external service dependencies

## Human Approval Status

- [x] Idea reviewed by human developer
- [x] Scope confirmed
- [x] Ready to proceed to Spec phase

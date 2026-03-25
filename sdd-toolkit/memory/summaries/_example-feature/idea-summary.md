# Summary: Idea — bulk-user-import

## Phase

Idea

## What Was Produced

A normalized idea document for adding bulk user import functionality via CSV file upload with validation, error reporting, and rollback support.

## Key Decisions

1. Scope limited to CSV format only — no Excel, JSON, or other formats in this iteration.
2. Import must be transactional — all-or-nothing with rollback on critical errors.
3. Error reporting must be row-level, not just pass/fail.

## Risks / Open Issues

- Maximum file size limit not yet confirmed with infrastructure team.
- Need to decide whether to support async processing for large files or synchronous only.

## What the Next Agent Must Know

- The feature is CSV-only, transactional, and requires row-level error reporting.
- Scope explicitly excludes real-time progress tracking and scheduled/recurring imports.
- The developer wants this to work for files up to at least 10,000 rows.

# Summary: Spec — bulk-user-import

## Phase

Spec

## What Was Produced

A complete specification for bulk user import with 6 functional requirements, 3 non-functional requirements, 5 acceptance criteria, and 8 edge cases documented.

## Key Decisions

1. CSV parsing happens server-side — no client-side pre-processing required.
2. Validation is two-phase: structural validation (headers, format) then business validation (duplicates, required fields, data types).
3. Error report is a downloadable CSV with row numbers, field names, and error descriptions.
4. Maximum file size set to 10MB (covers ~50,000 rows with typical user data).
5. Duplicate detection is by email address only.

## Risks / Open Issues

- Async processing deferred to a future iteration — synchronous only for now. This may be a problem for files near the 10MB limit.
- No spec for partial import (e.g., import valid rows, skip invalid ones). Currently all-or-nothing.

## What the Next Agent Must Know

- Two-phase validation: structural then business rules.
- All-or-nothing import with transactional rollback.
- Error report is a downloadable CSV, not just a screen message.
- Duplicate detection is email-based.
- Processing is synchronous in this iteration.

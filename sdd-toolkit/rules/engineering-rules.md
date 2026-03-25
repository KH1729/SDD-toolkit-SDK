# Engineering Rules

## Purpose

Standards that all implementation work must follow, regardless of the feature or task.

## Rules

### R-EN-01: Separation of Concerns
Workers implement. Validators review. Planners plan. No agent should perform work outside its defined role.

### R-EN-02: Tests Required
Every implementation task must include tests. Untested code is not considered complete.

### R-EN-03: No Dead Code
Do not generate code that is not used by the feature. Remove scaffolding, debugging artifacts, and unused imports before marking a task complete.

### R-EN-04: Consistent Style
Follow the project's existing code style and conventions. Do not introduce new patterns without explicit approval.

### R-EN-05: Error Handling
All code must handle errors explicitly. No silent failures, no empty catch blocks, no unhandled promises.

### R-EN-06: Clear Naming
Names (variables, functions, classes, files) must be descriptive and consistent with the codebase. Avoid abbreviations unless they are project-standard.

### R-EN-07: Minimal Dependencies
Do not introduce new external dependencies unless the task explicitly requires them. Prefer standard library solutions.

### R-EN-08: Documentation
Public APIs and non-obvious logic must be documented. Do not over-document obvious code.

### R-EN-09: Idempotent Operations
Where applicable, operations should be idempotent and safe to retry.

### R-EN-10: Security by Default
Never log secrets, never hardcode credentials, never trust external input without validation.

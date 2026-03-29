# Validation Rules

## Purpose

Standards for how validation is conducted and how findings are reported.

## Rules

### R-VA-01: Validators Review Only
Validation agents review and report. They do not fix, rewrite, or modify artifacts or code.

### R-VA-02: Explicit Pass/Fail
Every check must produce an explicit PASS or FAIL result. No ambiguous "looks okay" assessments.

### R-VA-03: Severity Classification
Every finding must be classified:
- **Info** — observation, no action required
- **Warning** — potential issue, should be reviewed
- **Error** — definite issue, must be fixed before approval
- **Critical** — blocking issue, requires immediate attention and likely rework

### R-VA-04: Evidence Required
Every finding must include evidence — what was checked, what was expected, what was found.

### R-VA-05: Spec Is Source of Truth
When validating implementation, the approved spec and design are the source of truth. Do not validate against assumptions or common practice — validate against what was specified.

### R-VA-06: Diff-Based Where Possible
Use diff-based comparison to minimize token consumption. Compare changes against approved artifacts rather than re-reading everything.

### R-VA-07: No Silent Approval
If a validator is uncertain about a check, it must flag the uncertainty rather than defaulting to PASS.

### R-VA-08: Validation Scope Must Be Defined
Before validation begins, the scope must be explicitly stated (what is being validated against what).

### R-VA-09: Cross-Phase Consistency
Validators should check consistency across phases — does the implementation match the design, does the design match the spec, does the spec match the idea?

### R-VA-10: Human Decides on Findings
The validator reports findings. The human developer decides what to do about them. Validators do not approve or reject.

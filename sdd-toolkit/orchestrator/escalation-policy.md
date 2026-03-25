# Escalation Policy

## Purpose

Define when agents must escalate a decision to the human developer rather than proceeding on their own.

## Mandatory Escalation Triggers

### 1. Ambiguity
**When:** A requirement, constraint, or instruction is ambiguous and could be reasonably interpreted in multiple ways.
**Action:** Stop work. Present the ambiguity and the possible interpretations to the developer. Do not guess.

### 2. Risky Tradeoffs
**When:** A design or implementation choice involves meaningful tradeoffs (performance vs. complexity, security vs. usability, etc.) where the right answer depends on business context.
**Action:** Present the tradeoff with pros/cons for each option. Let the developer decide.

### 3. Architecture Deviations
**When:** The current phase requires changes to previously approved architecture decisions, interface contracts, or scope boundaries.
**Action:** Stop work. Present the proposed deviation and its justification. The developer must approve before proceeding.

### 4. High Severity Validation Findings
**When:** Validation discovers a critical or high-severity issue that indicates a fundamental problem (not just a minor bug).
**Action:** Flag the finding immediately. Do not continue validating unrelated areas if the critical issue would invalidate them.

### 5. Cost / Token Concerns
**When:** A phase is consuming significantly more tokens than expected, or the remaining budget is insufficient for the remaining work.
**Action:** Report the situation. Propose options (compress output, reduce scope, stop and save budget for rework).

### 6. Conflicting Requirements
**When:** Two approved requirements or constraints contradict each other.
**Action:** Stop work. Present the conflict with references to both sources. The developer resolves the conflict.

### 7. Excessive Rework
**When:** A single phase has been rejected and reworked more than twice.
**Action:** Escalate. The repeated rework may indicate a deeper issue — unclear requirements, wrong approach, or misalignment.

## Escalation Format

When escalating, agents must provide:
1. **What** — clear description of the issue
2. **Why** — why this requires human input
3. **Options** — if applicable, the options with tradeoffs
4. **Recommendation** — if the agent has a recommendation, state it (but the human decides)
5. **Impact of delay** — what happens if this is not resolved promptly

# Token Efficiency Rules

## Goal

Maximize useful work per token spent. Every token should contribute to forward progress.

## Rules

### R-TE-01: Summary-First Reading
Agents must read summaries before full artifacts. Only load the full artifact if the summary is insufficient for the current task.

### R-TE-02: Scoped Context Loading
Load only the context relevant to the current phase and task. Do not load the entire feature history when only the previous phase's summary and handoff are needed.

### R-TE-03: Structured Handoffs
Use structured handoff documents to pass context between agents. This avoids the next agent having to re-read and re-analyze previous artifacts from scratch.

### R-TE-04: Diff-Based Validation
When validating, prefer diffing against the approved artifact rather than re-reading everything. Check what changed, not everything.

### R-TE-05: Compressed Summaries on Low Budget
When the token budget is tight, summaries should be shorter and more compressed. Prioritize essential information.

### R-TE-06: No Redundant Work
If an artifact has already been produced and approved, do not regenerate it. Reference the approved version.

### R-TE-07: Dynamic Scaling
Scale worker and validator counts based on actual complexity, not worst-case assumptions. For simple features, 1 worker and 1 validator is sufficient.

### R-TE-08: Sequential on Low Budget
When the token budget is low, execute tasks sequentially rather than in parallel. This reduces context duplication across workers.

### R-TE-09: Minimal Viable Output
Agents should produce the minimum output required by the template. Do not pad artifacts with filler content to appear thorough.

### R-TE-10: Early Termination on Blocking Issues
If a critical issue is found early in validation, report it immediately rather than completing all remaining checks. Save tokens for the rework cycle.

# Workflow Rules

## Canonical Flow

The required flow is:

**Idea → Spec → Design → Plan → Tasks → Implementation → Validation**

## Rules

### R-WF-01: No Phase Skipping
Every feature must pass through every phase in order. No phase may be skipped, even if it seems trivial.

### R-WF-02: No Phase Reordering
Phases execute in the canonical order. You may not start design before spec is approved, or implementation before tasks are approved.

### R-WF-03: Phase Outputs Required
Every phase must produce:
1. **Artifact** — the primary deliverable (spec, design, plan, etc.)
2. **Summary** — a concise summary written to shared memory
3. **Handoff** — a structured handoff document for the next agent
4. **State update** — updated feature state in memory

### R-WF-04: Approval Gates
In manual mode, every phase transition requires explicit human approval. No agent may self-approve a transition.

### R-WF-05: Rework Routing
If a phase is rejected, the workflow routes back to the responsible agent with specific feedback. Downstream artifacts from the rejected phase are invalidated.

### R-WF-06: Registry Snapshots
When a phase is approved, its artifact is versioned and stored in the registry. This snapshot is immutable.

### R-WF-07: Resumability
If a workflow is interrupted, it must be resumable from the last known state. Feature state in memory is the source of truth for workflow position.

### R-WF-08: Single Active Phase
Only one phase may be active per feature at a time. No parallel phase execution within a single feature.

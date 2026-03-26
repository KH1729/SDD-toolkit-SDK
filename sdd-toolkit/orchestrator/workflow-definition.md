# Workflow Definition

## Canonical Flow

```
Idea → Spec → Design → Plan → Tasks → Implementation → Validation
```
At any point in the workflow, if an escalation condition is triggered (as defined in `orchestrator/escalation-policy.md`), execution must pause and transition to an escalated state until resolved by the human developer.
Transitions between phases must be explicit. No implicit or automatic transitions are allowed without satisfying phase requirements and approval conditions.
Every feature must pass through this flow in order. No phase may be skipped or reordered.
Transitions between phases must be explicit. No implicit or automatic transitions are allowed without satisfying phase requirements and approval conditions.

## Phase Requirements

Each phase must emit **four valid outputs** before advancing:

| Output | Description | Location |
|---|---|---|
| **Artifact** | The primary deliverable for the phase | `workspace/<feature>/current/<phase>.md` |
| **Summary** | Concise summary for shared memory | `memory/summaries/<feature>/<phase>-summary.md` |
| **Handoff** | Structured context for the next agent | `memory/handoffs/<feature>/<from>-to-<to>.md` |
| **State Update** | Updated feature state | `memory/feature-state/<feature>.json` |

All outputs must be complete, consistent, and usable by the next phase. Partial or malformed outputs must not be accepted.

## Phase Completion Criteria

A phase is considered complete only if:

- All four required outputs are present
- The artifact is internally consistent and aligned with inputs
- The summary accurately reflects the artifact
- The handoff provides sufficient context for the next phase

If completeness is unclear, the phase must not advance and should trigger escalation.

After emitting all four, the workflow **pauses for human approval** (in manual mode).

## Phase Definitions

### 1. Idea
- **Agent:** Master Agent (capture) + Human (input)
- **Input:** Raw idea or request
- **Output:** Normalized idea document
- **Gate:** Human approves idea scope before proceeding

### 2. Spec
- **Agent:** Spec Agent
- **Input:** Approved idea summary
- **Output:** Bounded, testable specification
- **Gate:** Human approves spec before proceeding

### 3. Design
- **Agent:** Architecture Agent
- **Input:** Approved spec summary + handoff
- **Output:** Lean technical design
- **Gate:** Human approves design before proceeding

### 4. Plan
- **Agent:** Work Manager Agent
- **Input:** Approved design summary + handoff
- **Output:** Execution plan
- **Gate:** Human approves plan before proceeding

### 5. Tasks
- **Agent:** Work Manager Agent
- **Input:** Approved plan
- **Output:** Task breakdown + worker allocation proposal
- **Gate:** Human approves tasks and allocation before proceeding

### 6. Implementation
- **Agent:** Worker Agent(s)
- **Input:** Approved tasks + handoff
- **Output:** Implemented code, associated tests, and supporting documentation aligned with approved tasks
- **Gate:** Human reviews implementation before validation

### 7. Validation
- **Agent:** Validation Agent(s)
- **Input:** All approved artifacts + implementation
- **Output:** Validation report with pass/fail findings
- **Gate:** Human reviews findings and makes final decision
- If validation fails (critical or high severity findings), the workflow must return to the appropriate prior phase for rework before proceeding.

## Rework Behavior

If a phase is rejected by the human developer:

- The workflow returns to the corresponding draft phase
- Only the rejected concerns should be addressed (avoid full rework unless required)
- Downstream phases are invalidated if their inputs are affected

After rework, the phase must go through the same approval gate again.

## Approval Modes

| Mode | Behavior |
|---|---|
| **Manual** | Every gate requires explicit human approval |
| **Semi-Auto** | Low-risk gates auto-advance; high-impact gates require human approval |
| **Auto** | All gates auto-advance (developer accepts full responsibility) |

## State Machine Alignment

This workflow is executed through the state machine defined in `orchestrator/state-machine.md`.

Each phase corresponds to a set of states:
- Draft state
- Waiting for human approval state
- Approved state

Transitions between phases must follow the state machine rules.
# Workflow Definition

## Canonical Flow

```
Idea → Spec → Design → Plan → Tasks → Implementation → Validation
```

Every feature must pass through this flow in order. No phase may be skipped or reordered.

## Phase Requirements

Each phase must emit **four outputs** before advancing:

| Output | Description | Location |
|---|---|---|
| **Artifact** | The primary deliverable for the phase | `workspace/<feature>/current/<phase>.md` |
| **Summary** | Concise summary for shared memory | `memory/summaries/<feature>/<phase>-summary.md` |
| **Handoff** | Structured context for the next agent | `memory/handoffs/<feature>/<from>-to-<to>.md` |
| **State Update** | Updated feature state | `memory/feature-state/<feature>.json` |

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
- **Output:** Code, tests, documentation notes
- **Gate:** Human reviews implementation before validation

### 7. Validation
- **Agent:** Validation Agent(s)
- **Input:** All approved artifacts + implementation
- **Output:** Validation report with pass/fail findings
- **Gate:** Human reviews findings and makes final decision

## Approval Modes

| Mode | Behavior |
|---|---|
| **Manual** | Every gate requires explicit human approval |
| **Semi-Auto** | Low-risk gates auto-advance; high-impact gates require human approval |
| **Auto** | All gates auto-advance (developer accepts full responsibility) |

# State Machine

## States

The feature workflow is modeled as a state machine with explicit waiting gates.

```
IDEA_CAPTURED
    ↓
WAITING_FOR_HUMAN_IDEA_APPROVAL
    ↓ (approve)          ↘ (rework)
SPEC_DRAFTED              → back to IDEA_CAPTURED
    ↓
WAITING_FOR_HUMAN_SPEC_APPROVAL
    ↓ (approve)          ↘ (rework)
SPEC_APPROVED              → back to SPEC_DRAFTED
    ↓
DESIGN_DRAFTED
    ↓
WAITING_FOR_HUMAN_DESIGN_APPROVAL
    ↓ (approve)          ↘ (rework)
DESIGN_APPROVED            → back to DESIGN_DRAFTED
    ↓
PLAN_DRAFTED
    ↓
WAITING_FOR_HUMAN_PLAN_APPROVAL
    ↓ (approve)          ↘ (rework)
PLAN_APPROVED              → back to PLAN_DRAFTED
    ↓
TASKS_DRAFTED
    ↓
WAITING_FOR_HUMAN_TASKS_APPROVAL
    ↓ (approve)          ↘ (rework)
TASKS_APPROVED             → back to TASKS_DRAFTED
    ↓
IMPLEMENTATION_IN_PROGRESS
    ↓
WAITING_FOR_HUMAN_IMPLEMENTATION_APPROVAL
    ↓ (approve)          ↘ (rework)
IMPLEMENTATION_COMPLETE    → back to IMPLEMENTATION_IN_PROGRESS
    ↓
VALIDATION_IN_PROGRESS
    ↓
VALIDATION_REVIEW_REQUIRED
    ↓
WAITING_FOR_HUMAN_FINAL_DECISION
    ↓ (approve)          ↘ (rework → any prior state)
DONE
```
Note: At any point in the flow, an escalation condition may trigger a transition to `ESCALATED`. 
After resolution by the human developer, execution returns to the prior state.

## State Definitions

| State | Description | Allowed Transitions |
|---|---|---|
| `IDEA_CAPTURED` | Idea has been normalized and written | → `WAITING_FOR_HUMAN_IDEA_APPROVAL` |
| `WAITING_FOR_HUMAN_IDEA_APPROVAL` | Paused. Human must approve or rework. | → `SPEC_DRAFTED` (approve) / → `IDEA_CAPTURED` (rework) |
| `SPEC_DRAFTED` | Spec Agent has produced the spec | → `WAITING_FOR_HUMAN_SPEC_APPROVAL` |
| `WAITING_FOR_HUMAN_SPEC_APPROVAL` | Paused. Human must approve or rework. | → `SPEC_APPROVED` (approve) / → `SPEC_DRAFTED` (rework) |
| `SPEC_APPROVED` | Spec is approved and versioned | → `DESIGN_DRAFTED` |
| `DESIGN_DRAFTED` | Architecture Agent has produced the design | → `WAITING_FOR_HUMAN_DESIGN_APPROVAL` |
| `WAITING_FOR_HUMAN_DESIGN_APPROVAL` | Paused. Human must approve or rework. | → `DESIGN_APPROVED` (approve) / → `DESIGN_DRAFTED` (rework) |
| `DESIGN_APPROVED` | Design is approved and versioned | → `PLAN_DRAFTED` |
| `PLAN_DRAFTED` | Work Manager has produced the plan | → `WAITING_FOR_HUMAN_PLAN_APPROVAL` |
| `WAITING_FOR_HUMAN_PLAN_APPROVAL` | Paused. Human must approve or rework. | → `PLAN_APPROVED` (approve) / → `PLAN_DRAFTED` (rework) |
| `PLAN_APPROVED` | Plan is approved | → `TASKS_DRAFTED` |
| `TASKS_DRAFTED` | Work Manager has produced task breakdown | → `WAITING_FOR_HUMAN_TASKS_APPROVAL` |
| `WAITING_FOR_HUMAN_TASKS_APPROVAL` | Paused. Human must approve or rework. | → `TASKS_APPROVED` (approve) / → `TASKS_DRAFTED` (rework) |
| `TASKS_APPROVED` | Tasks approved, workers may begin | → `IMPLEMENTATION_IN_PROGRESS` |
| `IMPLEMENTATION_IN_PROGRESS` | Workers are executing tasks | → `WAITING_FOR_HUMAN_IMPLEMENTATION_APPROVAL` |
| `WAITING_FOR_HUMAN_IMPLEMENTATION_APPROVAL` | Paused. Human reviews implementation. | → `IMPLEMENTATION_COMPLETE` (approve) / → `IMPLEMENTATION_IN_PROGRESS` (rework) |
| `IMPLEMENTATION_COMPLETE` | All tasks done and approved | → `VALIDATION_COMPLETE` |
| `VALIDATION_IN_PROGRESS` | Validation agent is actively evaluating implementation | → `VALIDATION_REVIEW_REQUIRED` |
| `VALIDATION_REVIEW_REQUIRED` | Validation findings are ready for review | → `WAITING_FOR_HUMAN_FINAL_DECISION` |
| `WAITING_FOR_HUMAN_FINAL_DECISION` | Paused. Human makes final call. | → `DONE` (approve) / → any prior state (rework) |
| `ESCALATED` | Execution paused due to escalation condition requiring human decision | → return to previous state after resolution |
| `DONE` | Feature complete. All artifacts versioned. | Terminal state. |

## Rework Transitions

Rework can target any previous state. When rework rolls back past an approved phase, all downstream artifacts are invalidated and must be re-drafted after the reworked phase is re-approved.

## Invariants

- Only one state is active per feature at any time.
- `WAITING_FOR_HUMAN_*` states can only be exited by a human action (approve or rework).
- No agent may transition out of a waiting state programmatically in manual mode.
- Any state may transition to `ESCALATED` if an escalation condition is triggered.

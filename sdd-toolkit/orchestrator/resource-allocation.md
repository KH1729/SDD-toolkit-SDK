# Resource Allocation

## Goal

Maximize useful work while minimizing token use. Every worker and validator instance costs tokens. Scale only when the benefit outweighs the cost.

## Inputs for Scaling Decisions

| Input | Description |
|---|---|
| **Feature size** | Small (1-2 tasks), medium (3-6 tasks), large (7+ tasks) |
| **Task count** | Number of discrete implementation tasks |
| **Complexity** | Low (CRUD, boilerplate), medium (business logic, integrations), high (algorithms, security, data pipelines) |
| **Model capability** | What the underlying model can handle in a single context |
| **Token budget** | Hard ceiling on tokens for this feature (if set) |
| **Execution mode** | Manual, semi-auto, or auto |

## Task Independence Requirement

Parallel execution is allowed only when tasks are sufficiently independent.

Tasks are not independent if they:
- Modify the same files or components
- Depend on shared intermediate outputs
- Require the same large body of context to execute correctly

If task independence is unclear, default to sequential execution.

## Diminishing Returns Rule

Adding more workers or validators must provide meaningful benefit.

Do not increase parallelism when coordination overhead, duplicated context, or merge/review cost is likely to outweigh the benefit.

If adding another worker would mostly duplicate context rather than reduce elapsed work, do not add that worker.

## Scaling Guidelines

Scaling decisions must account for model capability.

If the selected model can reliably handle the relevant context and task load in a single pass, prefer fewer workers to avoid duplicated context and unnecessary token use.

### Small Feature (1-2 tasks, low complexity)
- Workers: **1**
- Validators: **1**
- Strategy: Sequential execution, single worker handles all tasks
- Summary compression: Normal

### Medium Feature (3-6 tasks, medium complexity)
- Workers: **2-3**
- Validators: **1**
- Strategy: Parallel tasks where dependencies allow
- Summary compression: Normal

### Large Feature (7+ tasks, high complexity)
- Workers: **3-5**
- Validators: **2-3**
- Strategy: Parallel execution with coordination checkpoints
- Summary compression: Normal (compressed if budget is tight)

## Validator Allocation Strategy

When multiple validators are used, validation must be split intentionally.

Preferred split options:
- By concern (for example: functional correctness, performance, security)
- By task group or artifact boundary

Avoid duplicate validation of the same artifact unless explicit redundancy is required.

## Low-Budget Rules (Priority Order)

When the token budget is constrained, apply the following in order:

1. **Sequential execution only.** No parallel workers. This eliminates duplicated context.
2. **Minimal workers.** Use one worker unless a second worker is clearly justified even under budget pressure.
3. **Minimal validators.** Use one validator regardless of feature size unless the developer explicitly approves more.
4. **Compressed summaries.** Summaries are shortened to essential facts only.
5. **Scoped validation.** Validate critical paths only, not exhaustive coverage.
6. **Single-pass preference.** Aim to get each phase right on the first try to avoid rework token cost.

## Allocation Decision Matrix

| Feature Size | Budget | Workers | Validators | Strategy |
|---|---|---|---|---|
| Small | Any | 1 | 1 | Sequential |
| Medium | Normal | 2-3 | 1 | Parallel where safe |
| Medium | Low | 1 | 1 | Sequential + compressed |
| Large | Normal | 3-5 | 2-3 | Parallel + checkpoints |
| Large | Low | 2 | 1 | Sequential + compressed |

## Dynamic Reallocation

If a phase fails validation, requires major rework, or shows coordination overhead, the allocation must be reassessed before retrying.

Reallocation guidelines:
- Reduce parallelism if the failure was caused by coordination or duplicated context
- Increase validation depth if major issues were missed
- Return to minimal allocation when the benefit of parallel execution is no longer clear

## Hard Limits

Resource allocation must respect the limits defined in `toolkit-config.yaml`.

Rules:
- Worker count must never exceed configured maximums
- Validator count must never exceed configured maximums
- If this document conflicts with configuration, configuration takes precedence

## Escalation Conditions

The Master Agent must escalate allocation decisions to the human developer when:
- Feature complexity and token budget suggest conflicting strategies
- Task independence is unclear
- Model capability is unknown or uncertain
- The recommended allocation would materially affect cost, speed, or validation depth

## Master Agent Responsibility

The Master Agent proposes the allocation based on these guidelines, the configured system limits, and the current token budget.

The proposed allocation is presented to the human developer for approval during the plan/tasks phase. The developer may override.

If conditions change during execution (for example: failed validation, rework, budget pressure, or coordination overhead), the Master Agent must reassess the allocation and escalate when required.

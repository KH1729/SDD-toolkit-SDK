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

## Scaling Guidelines

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

## Low-Budget Rules

When the token budget is constrained:

1. **Sequential execution only.** No parallel workers. This eliminates duplicated context.
2. **Compressed summaries.** Summaries are shortened to essential facts only.
3. **Minimal validators.** One validator regardless of feature size.
4. **Scoped validation.** Validate critical paths only, not exhaustive coverage.
5. **Single-pass preference.** Aim to get each phase right on the first try to avoid rework token cost.

## Allocation Decision Matrix

| Feature Size | Budget | Workers | Validators | Strategy |
|---|---|---|---|---|
| Small | Any | 1 | 1 | Sequential |
| Medium | Normal | 2-3 | 1 | Parallel where safe |
| Medium | Low | 1 | 1 | Sequential + compressed |
| Large | Normal | 3-5 | 2-3 | Parallel + checkpoints |
| Large | Low | 2 | 1 | Sequential + compressed |

## Master Agent Responsibility

The Master Agent proposes the allocation based on these guidelines. The allocation is presented to the human developer for approval during the plan/tasks phase. The developer may override.

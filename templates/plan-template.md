# Plan: {{title}}

This artifact is produced in workflow phase **Plan**, after **Design** is approved. Sections below labeled **Work Phases** are execution slices inside this document—not the toolkit workflow phases (Idea → Spec → Design → Plan → Tasks → Implementation → Validation).

**Design complete (upstream gate):** Approved `design.md` defines interfaces, boundaries, and tradeoffs; Plan does not invent new architecture.

**Task-ready (this plan):** Each work phase maps to the approved design, has clear goals and dependencies, and can be broken into tasks without guessing structure.

## Execution Strategy

{{How will this feature be implemented? Sequential tasks? Parallel workstreams? How many workers?}}

- Approach: sequential | parallel | mixed
- Estimated workers: 
- Estimated validators: 

## Work Phases

Ordered **execution** chunks for this feature (plan work phases). Do not confuse with workflow phase **Implementation**—that phase runs later, driven by approved `tasks.md`.

{{Break the implementation into ordered phases. Each phase should be independently completable.}}

### Phase 1: {{title}}
- **Goal:** 
- **Tasks:** 
- **Dependencies:** none | {{list}}
- **Validation checkpoint:** yes | no

### Phase 2: {{title}}
- **Goal:** 
- **Tasks:** 
- **Dependencies:** Phase 1
- **Validation checkpoint:** yes | no

## Dependency Map

{{Which tasks or phases depend on others? List blocking dependencies.}}

| Task/Phase | Depends On | Blocking? |
|---|---|---|
| | | |

## Integration Approach

{{How will the implemented pieces be integrated? All-at-once? Incremental?}}

- 

## Rollout Order

{{In what order should completed work be deployed or merged?}}

1. 

## Risks / Coordination Notes

{{Risks to the plan. Coordination needed between workers or with external teams.}}

- 

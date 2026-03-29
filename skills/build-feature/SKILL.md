# Build Feature Skill

## Purpose

Start and orchestrate the full Spec-Driven Development flow for a single feature.

## When to use

Use this skill when the user wants to:
- build a new feature
- start work on a feature from an idea
- generate the spec/design/plan/task flow for one feature
- continue a feature through the toolkit phases

## Inputs

- feature name
- feature idea or request
- optional business context
- optional constraints
- toolkit mode/config if relevant

## Behavior

This skill should:

1. Validate that the feature request is defined enough to start
2. Create or locate the feature workspace
3. Load toolkit configuration
4. Start from the correct phase:
   - idea
   - spec
   - design
   - plan
   - tasks
   - implementation
   - validation
5. Respect approval gates
6. Use the appropriate agent for the current phase
7. Save artifacts and summaries in the correct directories
8. Stop when human approval is required

## Required outputs

- feature workspace initialized or updated
- current phase artifacts created
- status summary
- next required approval or action

## Routing hints

Activate when the request contains phrases like:
- build feature
- start feature
- create feature
- work on feature
- continue feature
- implement feature through the toolkit

## Example requests

- "Build feature: user notifications"
- "Start a feature for invoice export"
- "Continue the checkout feature from design"
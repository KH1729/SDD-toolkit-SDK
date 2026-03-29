# Validate Feature Skill

## Purpose

Run validation for a feature and produce a review report.

## When to use

Use this skill when the user wants to:
- validate a feature
- review implementation against spec/design/plan
- generate a validation report
- check readiness for completion

## Inputs

- feature name
- validation target
- optional scope
- optional strictness level

## Behavior

This skill should:

1. Collect relevant feature artifacts
2. Load spec, design, plan, tasks, and implementation context
3. Compare implementation against intended behavior
4. Identify missing work, defects, or mismatches
5. Produce a validation report
6. Mark pass/fail or ready/not-ready
7. Recommend next action

## Required outputs

- validation report
- findings list
- pass/fail or readiness decision
- next step recommendation

## Routing hints

Activate when the request contains phrases like:
- validate feature
- review feature
- check if feature is complete
- run validation
- verify implementation

## Example requests

- "Validate the notifications feature"
- "Review the checkout feature against the spec"
- "Generate a validation report for export flow"
# Request Rework Skill

## Purpose

Send the current phase back for revision with clear feedback.

## When to use

Use this skill when the user wants to:
- reject the current output
- request changes
- send a phase back for rework
- provide review comments that must be incorporated

## Inputs

- feature or project name
- phase name
- rework reason
- requested changes
- optional priority or blocking notes

## Behavior

This skill should:

1. Verify the target artifact exists
2. Record that the phase was not approved
3. Save rework notes in a structured way
4. Keep the workflow in the same phase
5. Route the feedback to the responsible agent
6. Preserve approval/rework history
7. Prevent moving forward until the rework is resolved

## Required outputs

- rework recorded
- phase status updated
- feedback saved
- responsible next action identified

## Routing hints

Activate when the request contains phrases like:
- rework this
- needs changes
- send back
- revise this
- not approved
- fix these issues

## Example requests

- "Request rework on the architecture phase"
- "This plan needs changes before approval"
- "Send the spec back with these comments"
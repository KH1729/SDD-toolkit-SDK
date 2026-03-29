# Build Project Skill

## Purpose

Initialize or orchestrate the toolkit flow for a whole project, not just one feature.

## When to use

Use this skill when the user wants to:
- start a new project with the toolkit
- initialize project-wide structure
- define project-level specs, standards, and architecture
- bootstrap the toolkit for a new codebase

## Inputs

- project name
- project description
- optional architecture direction
- optional stack
- optional delivery constraints

## Behavior

This skill should:

1. Create or validate the project skeleton
2. Initialize toolkit folders if missing
3. Create the core project-level artifacts
4. Establish project conventions
5. Prepare the system for feature-by-feature work
6. Respect approval gates before advancing phases
7. Save summaries and project memory

## Required outputs

- initialized project structure
- project-level artifacts
- project status summary
- next phase recommendation

## Routing hints

Activate when the request contains phrases like:
- build project
- start project
- initialize project
- bootstrap project
- create new project with toolkit

## Example requests

- "Build a new project for CRM SaaS"
- "Initialize this repo with the SDD toolkit"
- "Set up a new project using the toolkit"
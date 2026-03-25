# Human Approval Rules

## Default Posture

The human developer is the final authority. All major decisions require human approval unless the developer has explicitly opted into a more autonomous mode.

## Rules

### R-HA-01: Manual Mode Is Default
The toolkit operates in manual approval mode by default. This is the safest and most predictable mode.

### R-HA-02: No Auto-Advancing
In manual mode, no agent may advance the workflow to the next phase without explicit human approval. The system must pause and wait.

### R-HA-03: No Silent Scope Changes
No agent may add, remove, or modify requirements, architecture decisions, or interface contracts without human review and approval.

### R-HA-04: No Silent Architecture Decisions
Architecture decisions are human-approved decisions. Agents may propose and recommend, but the human decides.

### R-HA-05: Agents Draft, Humans Decide
Agents may draft artifacts, make recommendations, analyze tradeoffs, and flag risks. They may NOT make final decisions on scope, architecture, or phase transitions.

### R-HA-06: Approval Is Recorded
Every approval (or rejection) is recorded in the feature state with a timestamp, the phase, and any notes from the developer.

### R-HA-07: Semi-Auto Mode
In semi-auto mode, low-risk phases (idea capture, task breakdown) may auto-advance, but high-impact phases (spec, design, final validation) still require human approval. The developer must explicitly enable this mode.

### R-HA-08: Auto Mode
In auto mode, all phases may auto-advance. This mode is only appropriate for low-risk, well-understood work. The developer must explicitly enable this mode and accepts full responsibility.

### R-HA-09: Mode Changes Require Approval
Switching from manual to semi-auto or auto mode requires explicit human action. Agents may not change the execution mode.

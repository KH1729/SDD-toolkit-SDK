# Architecture Rules

## Purpose

Constraints and standards for technical design decisions.

## Rules

### R-AR-01: Design Within Approved Scope
The design must implement the approved spec — no more, no less. Scope changes require human approval.

### R-AR-02: Lean Design
Design only what is needed. Do not build frameworks, abstractions, or extension points that the spec does not require.

### R-AR-03: Explicit Interfaces
All component boundaries must have explicitly defined interfaces. No implicit coupling through shared mutable state or global variables.

### R-AR-04: Data Flow Clarity
The data flow for every operation must be traceable from input to output. No hidden side effects.

### R-AR-05: Backwards Compatibility
Changes to existing interfaces must maintain backwards compatibility unless breaking changes are explicitly approved.

### R-AR-06: Stateless Where Possible
Prefer stateless components and operations. Where state is required, make it explicit, bounded, and observable.

### R-AR-07: Failure Isolation
Component failures should be isolated and should not cascade. Design for graceful degradation.

### R-AR-08: Observability
Every significant component must be observable — structured logging, health checks, and meaningful error messages at minimum.

### R-AR-09: No Premature Optimization
Design for correctness first. Optimize only when profiling identifies an actual bottleneck.

### R-AR-10: Security Boundaries
Clearly define trust boundaries. Validate all inputs crossing a trust boundary. Authentication and authorization checks must be explicit.

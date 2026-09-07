# Feature 04.4: Rules Engine and Match State

## What This Doc Covers

This doc defines the authoritative cricket rules system and match state machine.

It turns ball outcomes into runs, wickets, overs, innings, and results.

## Scope

Include:

- score logic
- dismissal logic
- over progression
- innings flow
- toss and result resolution
- AI fill behavior
- dead ball and reset behavior

Exclude:

- low-level ball physics
- batting and bowling animation
- UI layout
- multiplayer transport

## Implementation Tasks

1. Create a pure rules layer that can resolve a delivery without depending on presentation code.
2. Implement all required dismissal types and scoring cases.
3. Model innings and over transitions explicitly.
4. Separate state transitions from visual sequencing so replays and animations can follow the same rules.
5. Support AI fill so a match can still start when not all slots are human-controlled.
6. Add debug traces for each major state transition.

## Expected Output

- a complete match can start and finish
- scorekeeping is deterministic
- game state can be tested without the UI

## Dependencies

- Feature 04.1 ball physics
- Feature 04.2 batting outcomes
- Feature 04.3 bowling and fielding flow
- rules_engine_spec.md

## References

- [files/rules_engine_spec.md](/home/akash/Desktop/CRICKET/files/rules_engine_spec.md)
- [files/TDD.md](/home/akash/Desktop/CRICKET/files/TDD.md)

## AI Agent Tasks

- implement the state machine and transitions
- define dismissal and score resolution order
- document dead-ball and reset handling
- create pure tests for each rule path

## Tests

- wickets and extras score correctly
- innings and over transitions work
- AI fill does not break match start

## Exit Criteria

- the rules engine is the source of truth
- all major match states are explicit
- scoring and result logic are consistent and testable

# Feature 04.2: Batting Outcome and Shot Model

## What This Doc Covers

This doc defines the batting decision and outcome system.

The player should choose intent, timing, and direction, then the game should resolve that decision into a cricket result that is understandable and fair.

## Scope

Include:

- shot intent model
- timing windows
- power and placement
- defensive shots
- attacking shots
- mishits, edges, and misses
- run intent after contact

Exclude:

- bowling behavior
- full match rules
- UI layout
- camera definitions

## Implementation Tasks

1. Define how batting input maps to shot intent.
2. Separate timing quality from shot selection.
3. Make contact quality influence distance, direction, and dismissal risk.
4. Support defensive play as a valid outcome, not just a failed attack.
5. Ensure the player can clearly understand why a shot succeeded or failed.
6. Route all results through one outcome model so animation and scoring stay aligned.

## Expected Output

- batting that feels like a skill decision
- visible differences between timing outcomes
- a single place where batting results are resolved

## Dependencies

- Feature 02.2 batting animation set
- Feature 04.1 ball physics
- score and dismissal logic

## References

- [files/rules_engine_spec.md](/home/akash/Desktop/CRICKET/files/rules_engine_spec.md)
- [files/controller_handoff_spec.md](/home/akash/Desktop/CRICKET/files/controller_handoff_spec.md)

## AI Agent Tasks

- define the batting state and outcome table
- map input timing to result quality
- document each shot family and its purpose
- list all batting failure states

## Tests

- timing windows produce distinct outcomes
- defensive play does not always become a dismissal risk
- contact results match the selected shot family

## Exit Criteria

- batting is understandable and skill-based
- outcomes are consistent across similar inputs
- gameplay and animation use the same resolution model

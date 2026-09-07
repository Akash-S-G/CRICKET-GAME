# Feature 04.3: Bowling and Fielding Flow

## What This Doc Covers

This doc defines how bowling and fielding work together during a delivery sequence.

The purpose is to keep each ball cycle clear: choose delivery, release, resolve contact, hand off to fielding, and return the game to a stable state.

## Scope

Include:

- delivery choice and execution
- line and length control
- field setup
- fielding control handoff
- pickups, throws, catches, dives, and saves
- boundary and run-out pressure

Exclude:

- batting decision model
- full match scoring
- lobby or camera systems

## Implementation Tasks

1. Define the delivery sequence from run-up to post-ball recovery.
2. Create fielding roles and default positioning rules.
3. Build explicit handoff behavior when AI or human control changes.
4. Make fielding outcomes feed cleanly into scorekeeping and dismissal logic.
5. Add readable feedback for saves, drops, and misses.
6. Keep the sequence deterministic enough to debug and replicate.

## Expected Output

- complete delivery-to-fielding flow
- readable fielding outcomes
- smooth control handoff without state corruption

## Dependencies

- Feature 02.3 bowling animation set
- Feature 02.4 fielding motion
- Feature 04.1 ball physics

## References

- [files/controller_handoff_spec.md](/home/akash/Desktop/CRICKET/files/controller_handoff_spec.md)
- [files/rules_engine_spec.md](/home/akash/Desktop/CRICKET/files/rules_engine_spec.md)

## AI Agent Tasks

- define the bowling delivery sequence
- document the fielding handoff path
- map fielding actions to outcomes
- specify boundary save and catch handling

## Tests

- fielding handoff does not lose ball state
- throws and catches resolve correctly
- bowling-to-fielding transitions are stable

## Exit Criteria

- every delivery can complete cleanly
- fielding behavior is readable and consistent
- the match can continue after each ball without ambiguity

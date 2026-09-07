# Feature 04.1: Ball Physics and Contact Model

## What This Doc Covers

This doc defines the physical model for the cricket ball and how it interacts with bat, pitch, and field.

It is the simulation core that makes the game feel grounded instead of scripted.

## Scope

Include:

- ball trajectory
- swing, seam, spin, drag, and bounce behavior
- bat contact response
- pitch reaction
- boundary and stop behavior

Exclude:

- rule resolution
- batting shot choice logic
- UI or camera work
- presentation motion

## Implementation Tasks

1. Define the ball state model and update loop.
2. Model pitch response using configurable constants rather than magic numbers.
3. Create contact response rules for bat edge, middle, miss, and deflection.
4. Allow physics values to be tuned separately for pitch type, weather, and match mode.
5. Make the simulation deterministic enough for debugging and future multiplayer replication.
6. Expose debug output for trajectory, bounce, and contact outcome.

## Expected Output

- believable ball movement
- stable contact resolution
- tunable physics for balancing and realism

## Dependencies

- Feature 01 data contracts
- Feature 02 batting and bowling event hooks
- rules and match state systems

## References

- [files/delivery_physics_constants.json](/home/akash/Desktop/CRICKET/files/delivery_physics_constants.json)
- [files/rules_engine_spec.md](/home/akash/Desktop/CRICKET/files/rules_engine_spec.md)

## AI Agent Tasks

- define the ball state machine
- document the physics inputs and outputs
- list the tuning variables for pitch and delivery style
- add debug readouts for collision outcomes

## Tests

- identical inputs produce stable output
- pitch reactions match configured constants
- bat contact produces expected deflections

## Exit Criteria

- ball motion is predictable and tunable
- contact behavior feels cricket-specific
- the simulation can support batting and bowling systems

# Feature 02.3: Bowling Animation Set

## What This Doc Covers

This doc defines motion for bowlers and delivery actions.

Bowling must show run-up intent, release type, and follow-through so the batting player can read what is coming.

## Scope

Include:

- run-up styles
- gather and release poses
- pace and spin delivery motion
- variation markers for yorkers, slower balls, and spin changes
- follow-through and recovery

Exclude:

- batting clips
- fielding clips
- rules engine behavior
- input mapping

## Implementation Tasks

1. Create run-up motion that scales with pace or spin style.
2. Add release poses that make line, length, and delivery type visually legible.
3. Create variant releases for common bowling changes such as slower balls and wide-angle deliveries.
4. Add follow-through motion that does not collapse the body into an unreadable pose.
5. Support state interruptions for no-ball, dead-ball, and stopped deliveries.
6. Define event frames for ball release and post-release recovery.

## Expected Output

- believable bowling motion for different delivery types
- clear release frames for gameplay timing
- strong visual distinction between bowling styles

## Dependencies

- Feature 02.1 rig and locomotion
- Feature 04 bowling mechanics
- ball release timing hooks

## References

- [files/animation_requirements.md](/home/akash/Desktop/CRICKET/files/animation_requirements.md)
- [files/rules_engine_spec.md](/home/akash/Desktop/CRICKET/files/rules_engine_spec.md)

## AI Agent Tasks

- define bowling motion families and variant names
- document release timing expectations
- map delivery types to animation states
- identify interruption and recovery states

## Tests

- each delivery type plays the expected motion
- release events align with ball spawn
- follow-through does not break legibility

## Exit Criteria

- bowling feels tactical and readable
- motion supports both pace and spin gameplay
- the delivery release is clear to the batting player

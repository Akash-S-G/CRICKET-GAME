# Feature 02.2: Batting Animation Set

## What This Doc Covers

This doc defines all batting-related motion assets and rules.

Batting is the most expressive cricket action in the game, so the animation set must cover stance, timing quality, shot families, misses, and run-outs after the shot.

## Scope

Include:

- batting stance
- pre-shot anticipation
- defensive and attacking shot families
- timing quality variants
- follow-through
- batting misses and mistimed outcomes
- running between wickets motions related to batting

Exclude:

- bowling motion
- fielding or keeper motion
- rules logic
- camera behavior

## Implementation Tasks

1. Build the stance loop so the player looks ready before the ball arrives.
2. Create separate clip families for defensive, drive, cut, pull, sweep, lofted, and improvised shots.
3. Add timing variants so early, good, and late contact all read differently.
4. Ensure body weight, bat arc, and head direction communicate intent.
5. Add recovery states for misses, edges, and awkward follow-throughs.
6. Create transition clips for leaving the crease, turning for a run, and preparing the second run.

## Expected Output

- a complete batting motion library
- readable visual feedback for timing quality
- animation hooks that can trigger ball contact and shot resolution

## Dependencies

- Feature 02.1 rig and locomotion
- Feature 04 gameplay timing model
- bat contact and ball outcome events

## References

- [files/animation_requirements.md](/home/akash/Desktop/CRICKET/files/animation_requirements.md)
- [files/game_flow_and_camera_design.md](/home/akash/Desktop/CRICKET/files/game_flow_and_camera_design.md)

## AI Agent Tasks

- define clip names and motion categories for batting
- map timing states to animation variants
- document the contact event frame for each shot family
- list the recovery states after misses or poor timing

## Tests

- each shot family plays the correct clip
- timing variants are visually distinct
- bat contact events line up with gameplay resolution

## Exit Criteria

- batting feels intentional instead of generic
- the player can read shot quality from animation
- the batting system supports the gameplay timing model

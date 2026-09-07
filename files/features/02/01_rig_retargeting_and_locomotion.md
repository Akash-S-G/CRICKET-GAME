# Feature 02.1: Rig, Retargeting, and Locomotion

## What This Doc Covers

This doc defines the shared character motion foundation for the cricket game.

Before batting or bowling can feel believable, the game needs a stable humanoid rig, clean retargeting rules, and readable locomotion. This slice sets that foundation.

## Scope

Include:

- shared humanoid rig requirements
- retargeting compatibility rules
- locomotion clips
- pose correction and foot planting
- movement state plumbing

Exclude:

- shot-specific batting animations
- bowling release motion
- fielding-specific actions
- Timeline presentation work

## Implementation Tasks

1. Define the base rig assumptions for all player models.
2. Keep proportions and bone naming stable enough for multi-character retargeting.
3. Set locomotion states for idle, walk, jog, sprint, start, stop, turn, pivot, and recovery.
4. Add motion correction rules for foot alignment and body orientation.
5. Connect locomotion to gameplay speed, facing direction, and movement locks.
6. Document what clipping or distortion is acceptable and what must be corrected procedurally.

## Expected Output

- reusable motion foundation for every player character
- movement that reads clearly on mobile screens
- a shared rig standard for the entire animation team

## Dependencies

- Feature 01 data definitions for player and camera support
- chosen character art pipeline
- Animation Rigging package or equivalent

## References

- [files/animation_requirements.md](/home/akash/Desktop/CRICKET/files/animation_requirements.md)
- [files/animation_and_scene_pipeline_roadmap.md](/home/akash/Desktop/CRICKET/files/animation_and_scene_pipeline_roadmap.md)
- [files/TDD.md](/home/akash/Desktop/CRICKET/files/TDD.md)

## AI Agent Tasks

- specify the animation controller layout
- document the rig compatibility constraints
- define locomotion-to-gameplay parameter mapping
- add notes for pose correction and foot planting

## Tests

- all locomotion states transition cleanly
- retargeted clips maintain readable motion
- foot sliding is minimized in movement transitions

## Exit Criteria

- every player model can use the same motion system
- locomotion is stable enough to support batting and bowling
- movement is readable on mobile-sized screens

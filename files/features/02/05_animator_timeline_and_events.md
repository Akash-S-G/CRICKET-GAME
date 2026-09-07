# Feature 02.5: Animator, Timeline, and Animation Events

## What This Doc Covers

This doc explains how all animation clips connect to Unity runtime systems.

The purpose is to make animation not just exist, but drive gameplay cues in a controlled and maintainable way.

## Scope

Include:

- animator controller structure
- blend tree conventions
- Timeline usage
- animation events
- state machine parameters

Exclude:

- clip production details
- gameplay mechanics
- camera logic
- UI layout

## Implementation Tasks

1. Define separate controllers or layers for locomotion, batting, bowling, fielding, keeper, and presentation.
2. Keep parameter names stable and documented.
3. Use blend trees only where motion variation is continuous and readable.
4. Use Timeline for non-interactive presentation, replays, and scripted entrances.
5. Define animation events for bat contact, ball release, catch point, stump break, and celebration trigger.
6. Document which events are authoritative and which are only visual.

## Expected Output

- a maintainable animation runtime structure
- reliable gameplay event hooks
- predictable transitions between motion states

## Dependencies

- Feature 02.1 through 02.4 motion content
- Unity Animator and Timeline packages
- gameplay event pipeline

## References

- [files/animation_and_scene_pipeline_roadmap.md](/home/akash/Desktop/CRICKET/files/animation_and_scene_pipeline_roadmap.md)
- [files/animation_requirements.md](/home/akash/Desktop/CRICKET/files/animation_requirements.md)

## AI Agent Tasks

- define animator parameter lists
- document the layer and state layout
- map animation events to gameplay callbacks
- explain Timeline ownership boundaries

## Tests

- controller transitions are deterministic
- events fire at the expected frames
- Timeline sequences do not block gameplay logic

## Exit Criteria

- animation can be controlled by gameplay code without hacks
- the team has a single documented event contract
- presentation motion is cleanly separated from live control

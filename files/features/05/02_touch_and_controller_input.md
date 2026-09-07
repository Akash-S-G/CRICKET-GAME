# Feature 05.2: Touch and Controller Input

## What This Doc Covers

This doc defines how the player actually interacts with the game.

The input system must be mobile-first, but it should also support controller input for broader device coverage and testing.

## Scope

Include:

- touch gestures
- buttons and on-screen controls
- controller mapping
- aim and timing input
- accessibility-friendly input behavior

Exclude:

- camera design
- tutorial sequencing
- rules logic
- animation implementation

## Implementation Tasks

1. Define the minimum touch controls needed for batting, bowling, navigation, and menus.
2. Keep touch regions large enough for one-handed mobile play where possible.
3. Map controller inputs to the same action model so the game can be tested on different devices.
4. Avoid gesture ambiguity by separating tap, hold, drag, and swipe semantics.
5. Add input buffering and timing windows where they improve playability.
6. Document what happens when inputs are missing, delayed, or interrupted.

## Expected Output

- clear and consistent mobile controls
- usable controller support
- input behavior that matches the gameplay model

## Dependencies

- Feature 05.1 camera modes
- Feature 04 batting and bowling actions
- mobile UI layout

## References

- [files/controller_handoff_spec.md](/home/akash/Desktop/CRICKET/files/controller_handoff_spec.md)
- [files/game_flow_and_camera_design.md](/home/akash/Desktop/CRICKET/files/game_flow_and_camera_design.md)

## AI Agent Tasks

- define the action map and input bindings
- document the touch zones and gestures
- specify controller fallback behavior
- explain how input timing is buffered

## Tests

- touch input triggers the correct action
- controller mapping mirrors touch behavior
- accidental taps do not trigger critical actions

## Exit Criteria

- the controls are understandable without a tutorial
- the same action model works across devices
- input feels reliable on mobile

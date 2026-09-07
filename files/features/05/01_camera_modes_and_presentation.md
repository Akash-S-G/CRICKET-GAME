# Feature 05.1: Camera Modes and Presentation

## What This Doc Covers

This doc defines the camera system for the mobile-first cricket game.

The camera is a gameplay communication tool. It must show the right cricket action at the right time, and it must support future presentation modes without making the controls confusing.

## Scope

Include:

- batting camera modes
- bowling camera modes
- fielding camera modes
- replay and cinematic cameras
- first-person and other special views
- camera transitions and cuts

Exclude:

- touch controls
- tutorial flow
- gameplay rules
- UI widgets

## Implementation Tasks

1. Define camera modes for batting, bowling, fielding, keeper, replay, and menu presentation.
2. Add a first-person or close-follow mode only where it improves readability and does not harm control clarity.
3. Create rules for when the camera follows the ball, follows the player, or frames the broader action.
4. Add smooth transitions for mode switches so the player is never disoriented.
5. Build camera presets that can be driven by match state and delivery context.
6. Document how each camera mode helps the player understand the game.

## Expected Output

- a clear camera language for the whole game
- support for immersive and broadcast-style views
- camera behavior that is stable on mobile devices

## Dependencies

- Feature 03 scene and UI flow
- Feature 04 gameplay states
- feature data for camera presets

## References

- [files/game_flow_and_camera_design.md](/home/akash/Desktop/CRICKET/files/game_flow_and_camera_design.md)
- [files/scene_by_scene_setup.md](/home/akash/Desktop/CRICKET/files/scene_by_scene_setup.md)

## AI Agent Tasks

- define the camera mode list
- document each mode's purpose and trigger
- specify the transition rules between modes
- note which views are mandatory for MVP

## Tests

- camera transitions are not abrupt
- each mode frames the intended action
- mobile view remains readable in all core states

## Exit Criteria

- camera behavior supports cricket readability
- special modes do not confuse the player
- the game can show both gameplay and presentation clearly

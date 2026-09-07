# Feature 03.1: Boot, Login, and Home Flow

## What This Doc Covers

This doc defines the first-run path from app launch to the main home screen.

It must be simple, fast, and safe on mobile so the player always knows whether the game is loading, authenticating, restoring state, or ready to play.

## Scope

Include:

- boot scene
- splash and loading states
- login or guest profile setup
- profile restore
- home menu
- navigation out of home

Exclude:

- tutorial details
- gameplay HUD
- replay and results screens
- matchmaking internals

## Implementation Tasks

1. Create a boot flow that checks save data, content readiness, and service availability.
2. Add a profile entry path that supports either signed-in or guest-first behavior.
3. Build a home screen that clearly shows the current mode and the next action.
4. Make navigation state-driven instead of hard-coded to button callbacks.
5. Add loading and failure states that never trap the player in a blank screen.

## Expected Output

- a predictable first-launch experience
- safe recovery when data is missing
- a home screen that works as the central hub

## Dependencies

- Feature 01.2 profile and session services
- Feature 01.3 remote config and analytics
- UI style guidelines

## References

- [files/scene_by_scene_setup.md](/home/akash/Desktop/CRICKET/files/scene_by_scene_setup.md)
- [files/game_flow_and_camera_design.md](/home/akash/Desktop/CRICKET/files/game_flow_and_camera_design.md)

## AI Agent Tasks

- define scene boot order
- specify the loading and fallback states
- document the home screen entry logic
- map out which actions are available from home

## Tests

- app boots to a valid state with good save data
- app handles missing or invalid save data safely
- home screen navigation is stable

## Exit Criteria

- the player can launch the game and reach the home screen consistently
- there is no ambiguous startup state
- all startup failures have a visible recovery path

# Feature 03.4: Customize and Online Lobby

## What This Doc Covers

This doc defines the non-match personalization and future online entry points.

It is intentionally split from the main gameplay flow so cosmetic and multiplayer surfaces do not pollute the core match experience.

## Scope

Include:

- player and team customization
- kit, bat, and profile visual settings
- online lobby shell
- future match entry and readiness states
- room or session presentation

Exclude:

- gameplay rules
- tutorial flow
- in-match HUD
- network authority details

## Implementation Tasks

1. Build a customization UI that uses data-driven options rather than hard-coded layouts.
2. Keep cosmetic changes separate from gameplay-affecting state.
3. Create an online lobby shell that can show session status, readiness, and match entry.
4. Add placeholder flows for unsupported online states so the UI remains future-proof.
5. Make sure customization is not required before first play.

## Expected Output

- a clean personalization experience
- an online-ready entry screen
- no coupling between cosmetics and core match rules

## Dependencies

- Feature 01 profile and save services
- Feature 07 multiplayer readiness
- UI asset pipeline

## References

- [files/system_design.md](/home/akash/Desktop/CRICKET/files/system_design.md)
- [files/competitive_analysis.md](/home/akash/Desktop/CRICKET/files/competitive_analysis.md)

## AI Agent Tasks

- define customization data categories
- outline lobby states and placeholders
- document what is cosmetic versus functional
- specify the room readiness presentation

## Tests

- customization changes persist correctly
- lobby state renders safely when multiplayer is unavailable
- cosmetic edits do not alter gameplay logic

## Exit Criteria

- the player can personalize the game without friction
- the lobby path is ready for online expansion
- customization stays isolated from match simulation

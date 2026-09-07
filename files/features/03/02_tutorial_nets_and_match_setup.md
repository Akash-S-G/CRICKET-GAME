# Feature 03.2: Tutorial, Nets, and Match Setup

## What This Doc Covers

This doc defines the learning and pre-match preparation flow.

The player should learn the game in a controlled environment before entering a real match. Nets and setup screens should teach, not overwhelm.

## Scope

Include:

- tutorial scene flow
- practice nets
- match setup
- side selection
- kit or team selection
- difficulty and mode selection

Exclude:

- home menu structure
- in-match HUD
- replay and results
- online lobby specifics

## Implementation Tasks

1. Build a tutorial path that introduces one mechanic at a time.
2. Create a nets environment where players can practice without full match pressure.
3. Define a match setup screen that clearly shows mode, opponents, and match settings.
4. Keep the setup flow mobile-friendly with few taps and strong visual hierarchy.
5. Add a skip or return path so advanced players are not forced through onboarding every time.

## Expected Output

- a learnable game entry path
- a practice space before full matches
- a clean setup interface for match selection

## Dependencies

- Feature 03.1 boot and home flow
- Feature 05 tutorial and camera design
- Feature 04 gameplay systems for practice interaction

## References

- [files/player_onboarding_and_tutorial_flow.md](/home/akash/Desktop/CRICKET/files/player_onboarding_and_tutorial_flow.md)
- [files/scene_by_scene_setup.md](/home/akash/Desktop/CRICKET/files/scene_by_scene_setup.md)

## AI Agent Tasks

- define the tutorial sequence and gating
- describe the nets interaction model
- document the match setup screen fields
- list all skip and return behaviors

## Tests

- tutorial steps unlock in the correct order
- nets allows practice without progression side effects
- setup changes produce the correct match configuration

## Exit Criteria

- a new player can learn the core actions
- experienced players can jump to practice or match setup quickly
- the pre-match flow is easy to understand on mobile

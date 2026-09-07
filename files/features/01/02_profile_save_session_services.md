# Feature 01.2: Profile, Save, and Session Services

## What This Doc Covers

This doc describes the persistent service layer for player profile data, save/load behavior, and session lifecycle management.

The purpose is to make the game state durable and predictable across app launches, device restarts, and future online session transitions.

## Scope

Include:

- profile service
- local save service
- session controller
- restore and resume flow
- offline-first state handling

Exclude:

- remote config
- analytics transport
- matchmaking
- gameplay simulation

## Implementation Tasks

1. Define the player profile model for identity, unlocks, settings, and progression.
2. Build a save pipeline that writes atomically and can recover from corrupted files.
3. Create a session lifecycle that tracks the current run, mode selection, match state, and resume behavior.
4. Separate temporary match state from long-lived profile state.
5. Support a safe fallback path when save data is missing or outdated.
6. Add clear ownership rules for which system is allowed to write which fields.

## Expected Output

- local persistence for profile and progression
- deterministic session creation and teardown
- resume support after app relaunch
- explicit boundaries between profile, session, and match runtime state

## Dependencies

- Feature 01.1 data contracts and schemas
- Unity persistence APIs
- UI flow that can show loading and recovery states

## References

- [files/system_design.md](/home/akash/Desktop/CRICKET/files/system_design.md)
- [files/development_plan.md](/home/akash/Desktop/CRICKET/files/development_plan.md)
- [files/TDD.md](/home/akash/Desktop/CRICKET/files/TDD.md)

## AI Agent Tasks

- define the profile service interface
- implement load, save, and reset flows
- add corruption recovery and backup writing
- document the session ownership model

## Tests

- save and load round trip works
- corrupted save falls back safely
- profile version migration works
- session state is restored only when valid

## Exit Criteria

- The app can boot, save progress, close, and reopen without losing valid data
- session transitions do not corrupt profile state
- save behavior is predictable under failure

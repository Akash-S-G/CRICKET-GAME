# Feature 07.2: Lobby and Session Flow

## What This Doc Covers

This doc defines the user-facing path into a multiplayer session.

The goal is to make the lobby, room, and match entry flow clear enough that future online cricket feels organized rather than experimental.

## Scope

Include:

- lobby states
- room join and ready flow
- host or session leader behavior
- pre-match loading
- session start and return flow

Exclude:

- low-level replication
- gameplay rules
- mobile settings
- tutorial flow

## Implementation Tasks

1. Design lobby states that are readable on mobile.
2. Show who is present, who is ready, and when the session can begin.
3. Keep session joining and leaving predictable.
4. Add loading and reconnect presentation for unstable connections.
5. Ensure the lobby does not overlap confusingly with single-player flows.

## Expected Output

- a clear multiplayer entry experience
- session flow that can support future matchmaking
- lobby states that are easy to debug

## Dependencies

- Feature 07.1 authority model
- Feature 03 UI framework
- future networking provider

## References

- [files/system_design.md](/home/akash/Desktop/CRICKET/files/system_design.md)
- [files/competitive_analysis.md](/home/akash/Desktop/CRICKET/files/competitive_analysis.md)

## AI Agent Tasks

- define the lobby state diagram
- document the ready and start conditions
- map join, leave, and reconnect flows
- specify the data shown in the room UI

## Tests

- lobby state changes are stable
- session start only occurs when conditions are met
- reconnect presentation does not break the flow

## Exit Criteria

- multiplayer entry is understandable
- lobby and session transitions are explicit
- the UI can scale to online play later

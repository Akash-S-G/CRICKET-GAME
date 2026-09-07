# State Machines and Event Model

## Purpose

This doc defines the application state machines and the event system that connects the domain, UI, and presentation layers.

## State Machines

### 1. App State

States:

- `Boot`
- `Auth`
- `Home`
- `Loading`
- `InMatch`
- `Results`
- `OfflineFallback`
- `ErrorRecovery`

Transitions:

- `Boot -> Auth`
- `Auth -> Home`
- `Home -> Loading`
- `Loading -> InMatch`
- `InMatch -> Results`
- `Any -> OfflineFallback` on content or service failure
- `Any -> ErrorRecovery` on unrecoverable local corruption

### 2. Match State

States:

- `NotStarted`
- `TossPhase`
- `Innings1`
- `InningsBreak`
- `Innings2`
- `Complete`

### 3. Delivery State

States:

- `Bowling`
- `InFlight`
- `BatterAction`
- `BallDead`
- `Resolved`

### 4. Player State

States:

- `Idle`
- `Batting`
- `Bowling`
- `Fielding`
- `Transitioning`
- `Disconnected`

### 5. Camera State

States:

- `Default`
- `Override`
- `Replay`
- `Cutscene`
- `Training`

## Event Model

Use a central domain event stream or event bus with explicit payload types.

Recommended approach:

- C# events or a strongly typed message bus in the domain layer
- no string-only broadcast system
- no scene-local hidden events for critical match logic

## Required Events

- `OnBootStarted`
- `OnAuthResolved`
- `OnContentReady`
- `OnMatchStarted`
- `OnTossComplete`
- `OnBallDelivered`
- `OnShotPlayed`
- `OnWicketFallen`
- `OnOverComplete`
- `OnInningsEnded`
- `OnRewardGranted`
- `OnSaveConflict`
- `OnHandoffTriggered`
- `OnHandoffConfirmed`

## Ordering Rules

1. Domain events are emitted before presentation reacts.
2. UI should respond after state mutation is complete.
3. Animation events may trigger gameplay only if they are authorized by the domain.
4. Network replication should carry domain state, not presentation state.

## Payload Rules

- payload types must be explicit classes or structs,
- timestamps should use UTC or tick count,
- payloads must not depend on scene references,
- event ordering must be deterministic in a single tick.

## Dependencies

- rules engine
- controller handoff
- UI architecture

## Tests

- state transitions are valid and complete
- event payloads are serializable
- ordering is deterministic within a tick

## Exit Criteria

- every important state transition is explicit
- the event system is typed and testable
- UI and gameplay do not poll each other for core state


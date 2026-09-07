# Feature 07: Multiplayer Readiness

## 1. What This Feature Is

This feature prepares the game for online multiplayer.

It includes:

- local/LAN proof,
- server-authoritative planning,
- AI backfill,
- control handoff under latency,
- disconnect/reconnect behavior,
- session ownership,
- network-safe rules and state,
- session and lobby flow.

## 2. Why This Feature Matters

Multiplayer should not be bolted on later.

The game must already be shaped in a way that can support:

- authoritative match resolution,
- fair gameplay,
- stable control ownership,
- predictable session flow,
- mobile-friendly latency handling.

## 3. Multiplayer Architecture

Detailed contracts:

- [Networking go/no-go and budget](07/00_networking_go_no_go_and_budget.md)
- [Authority matrix and replication table](07/04_authority_matrix_and_replication_table.md)

### 3.1 Authority Model

Use the server as the source of truth for:

- ball state,
- dismissals,
- scoring,
- match progress,
- fielding ownership,
- final outcomes.

Clients may predict:

- local movement,
- immediate control response,
- presentation.

### 3.2 AI Backfill

The system must support missing players by:

- filling empty slots with AI,
- preserving the match flow,
- avoiding stalled lobbies,
- keeping the game playable even when the lobby is incomplete.

### 3.3 Control Handoff

The fielding handoff system must:

- transfer ownership cleanly,
- preserve physical continuity,
- handle latency,
- support disconnect/reconnect.

## 4. Session and Lobby Flow

### 4.1 Lobby Behavior

- create lobby,
- join lobby,
- mark ready,
- assign roles,
- handle match start,
- handle match exit,
- support fallback to offline or AI fill.

### 4.2 Session Flow

- create session,
- attach players,
- assign authority,
- begin match,
- replicate important events,
- end match,
- clean up session state.

## 5. Implementation Tasks

### 5.1 Small Multiplayer Proof

- implement local/LAN play,
- verify two-player synchronization,
- verify handoff behavior,
- verify session start and end.

### 5.2 Latency Behavior

- test artificial latency,
- preserve readability,
- avoid snapping,
- make state correction visually soft but logically authoritative.

### 5.3 Future-Ready Architecture

- keep game logic compatible with dedicated server assumptions,
- keep offline and online rule paths aligned,
- avoid client-only systems that will break later.

## 6. Expected Output

- Small multiplayer matches work.
- Handoff remains readable.
- Future Main Mode networking does not need a rewrite.
- The game architecture can scale toward authoritative online cricket.

## 7. Dependencies

- Feature 04 core gameplay
- Feature 06 mobile MVP
- `controller_handoff_spec.md`
- `TDD.md`
- `system_design.md`

## 8. References

- [controller_handoff_spec.md](../controller_handoff_spec.md)
- [TDD.md](../TDD.md)
- [system_design.md](../system_design.md)
- [development_plan.md](../development_plan.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- build lobby scaffolds,
- build network state flow,
- build ownership transfer logic,
- build latency test hooks,
- build disconnect/reconnect handling,
- build AI backfill behavior,
- build multiplayer debug readouts.

## 10. Expected Tests

- multiplayer join/start tests,
- handoff latency tests,
- disconnect/reconnect tests,
- state sync tests,
- AI backfill tests,
- session cleanup tests.

## 11. Exit Criteria

- Two clients complete 20 consecutive deliveries without score, wicket, or innings divergence.
- The proof survives 100 ms one-way delay and 2% packet loss without stalling.
- Handoff remains recoverable at 150 ms one-way delay.
- Average gameplay traffic stays at or below 12 KB/s per player, with a 24 KB/s hard ceiling.
- Duplicate or reordered events cannot duplicate scoring, presentation, or rewards.

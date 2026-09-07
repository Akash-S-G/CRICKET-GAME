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

### 3.1 Authority Model (server-wins `TDD.md:33` + `system_design/physics_tick_and_reconciliation.md:16`)

Server (NGO host/dedicated `TDD.md:9`) owns ball, dismissals, scoring, progress, ownership, result `features/07/01_authority_and_replication_model.md:16` + `PhysicalState.DeliveryId/ServerTick` `TDD.md:58`. Version NGO 2.4.x `TDD.md:9`, Fusion fallback only at Main Mode `TECH_STACK.md:15`.

Clients predict local movement/control, reconcile via snapshots `system_design/physics_tick_and_reconciliation.md:24` blending non-critical, snap for rule-critical `system_design/physics_tick_and_reconciliation.md:24`. Anim intent replicates `shotType/timingQuality` `TDD.md:41` not transforms `TDD.md:41`.

| System | Authoritative | Replicates | Rate |
|---|---|---|---|
| Ball | Server | pos/vel/DeliveryId | 30Hz fixed `TDD.md:36` sub-step |
| Shot | Server contact | `shotType/timingQuality/shotPower` | event |
| Fielding handoff | Server | `HandoffRequestRPC` with `PhysicalState` `TDD.md:58` | immediate |
| Match state | Server | `NotStarted->Complete` `system_design/state_machines_and_event_model.md:28` | event |
| Animation | Client owns evaluation | `AnimationStateId/NormalizedTime` | render-rate `TDD.md:93` |

### 3.2 AI Backfill

The system must support missing players by:

- filling empty slots with AI,
- preserving the match flow,
- avoiding stalled lobbies,
- keeping the game playable even when the lobby is incomplete.

### 3.3 Control Handoff (explicit RPC `TDD.md:40` + `controller_handoff_spec.md:41` + `features/07/03_disconnect_reconnect_and_backfill.md:16`)

- Trigger `handoff_radius_m` `mode_config_schema.json:18` 3.5/5.0 intercept-time priority `controller_handoff_spec.md:35` + `GDD.md:222`.
- Confirm `HandoffRequestRPC`->`HandoffConfirmedRPC` `controller_handoff_spec.md:42`, blend 0.15s `TDD.md:75` during `TransitioningToHuman` `TDD.md:50`, server `Transitioning` authoritative `system_design/state_machines_and_event_model.md:33`.
- Interest mgmt `TDD.md:39` cull distant fielders; bandwidth biggest risk `risks.md:14` revisit at Main Mode milestone `system_design/build_pipeline.md:16` CI ded-server build `features/08_testing_ci_and_release.md:16`.

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

## 10. Expected Tests (latency + authority per `system_design/physics_tick_and_reconciliation.md:24`)

- Handoff latency artificially injected 100-150ms `risks.md:15`, verify blend not snap.
- Deterministic ball `features/04/01_ball_physics_and_contact_model.md:21` same inputs same output `system_design/physics_tick_and_reconciliation.md:24`.
- Auth: spoofed `OnWicketFallen` rejected `system_design/security_threat_model.md:33` server owns.
- AI backfill `features/07/03_disconnect_reconnect_and_backfill.md:16` missing slot -> `AIController` `TDD.md:41` without stall `mode_config_schema.json:18` `min_players_to_start`.

## 11. Exit Criteria

- Two clients complete 20 consecutive deliveries without score, wicket, or innings divergence.
- The proof survives 100 ms one-way delay and 2% packet loss without stalling.
- Handoff remains recoverable at 150 ms one-way delay.
- Average gameplay traffic stays at or below 12 KB/s per player, with a 24 KB/s hard ceiling.
- Duplicate or reordered events cannot duplicate scoring, presentation, or rewards.

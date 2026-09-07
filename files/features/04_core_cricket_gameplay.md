# Feature 04: Core Cricket Gameplay

## 1. What This Feature Is

This feature implements the actual cricket simulation and rules. It is the heart of the game.

It includes:

- ball physics
- batting timing
- shot selection
- bowling execution
- fielding control handoff
- wickets
- extras
- overs
- innings
- scorekeeping
- match state flow
- AI fill behavior

## 2. Why This Feature Matters

This feature decides whether the game feels like cricket.

If this is wrong:

- the timing will feel fake,
- the fielding will feel unresponsive,
- the scoring will feel wrong,
- the match flow will feel broken,
- the game will not be competitive or believable.

## 3. Simulation Design

### 3.1 Ball Physics (30Hz fixed + sub-step, server authoritative `TDD.md:33` `system_design/physics_tick_and_reconciliation.md:8`)

- State `BallState { pos, vel, angVel, seamAngle, DeliveryId }` tick `30Hz` min `TDD.md:36` 60Hz if perf, sub-step on fast `system_design/physics_tick_and_reconciliation.md:16` bat-ball contact.
- Forces: drag `delivery_physics_constants.json:7` `drag 0.47`, swing `0.35m` `delivery_physics_constants.json:12`, pitch COR `0.65` `delivery_physics_constants.json:24` * `pitch_schema.json:4` modifier.
- Deterministic `system_design/physics_tick_and_reconciliation.md:24` same tick order, seed explicit, snapshot `TDD.md:58` `PhysicalState.ServerTick`.

### 3.2 Batting (timing 60/120ms `delivery_physics_constants.json:20` -> blend `timingQuality`)

- Intent late: `ShotIntent` enum `features/04/02_batting_outcome_and_shot_model.md:16` defensive/drive/cut/pull/sweep/loft + `ShotDirection` 360 deg `GDD.md:165` maps to `animation_and_scene_pipeline_roadmap.md:139` `shotType/timingQuality/shotPower`.
- Timing `good 60ms` `early/late 120ms` `delivery_physics_constants.json:20` good->sweet `COR 0.85` edge `0.55` `delivery_physics_constants.json:17` + variance `25deg` `delivery_physics_constants.json:22`.
- Mishits via `TDD.md:41` replicated intent `timingQuality` [-1/0/1] -> `animation_clip_inventory.md:114` `bat_loft_early/good/late`.

### 3.3 Bowling

Bowling must support:

- delivery type selection,
- release timing,
- line and length,
- accuracy quality,
- pace/spin variation,
- follow-through and recovery.

Bowling should feel controlled and tactical.

### 3.4 Fielding (explicit RPC handoff `TDD.md:40` `controller_handoff_spec.md:41`)

- Handoff via `HandoffRequestRPC`/`HandoffConfirmedRPC` `controller_handoff_spec.md:42` with `PhysicalState` `TDD.md:58` `AnimationStateId/NormalizedTime/DeliveryId` + `handoff_radius_m` `mode_config_schema.json:18` 3.5 Main /5.0 Gully, priority intercept time `controller_handoff_spec.md:35` + `GDD.md:222`.
- Blend 0.15s `TDD.md:75` during `TransitioningToHuman` `TDD.md:50` `IPlayerController.State`, server already authoritative `system_design/state_machines_and_event_model.md:33` `OnHandoffTriggered/Confirmed`.
- Low tier reactive only `TDD.md:81` no dives.

## 4. Match Rules

### 4.1 Score Logic

The rules engine must resolve:

- runs,
- dots,
- boundaries,
- extras,
- wickets,
- over counts,
- innings counts,
- chase targets.

### 4.2 Dismissals

Implement:

- bowled,
- caught,
- LBW,
- run-out,
- stumped,
- hit wicket.

### 4.3 Match Flow

Implement:

- toss,
- innings start,
- over progression,
- strike rotation,
- bowler change,
- wicket fall,
- innings end,
- result resolution.

### 4.4 AI Fill

If human players are missing:

- AI should fill slots,
- match start should not stall,
- AI should preserve the intended role or behavior of the slot.

## 5. Implementation Tasks

### 5.1 Domain Model (pure per `features/04/04_rules_engine_and_match_state.md:16` + `system_design/state_machines_and_event_model.md:33`)

- Pure `RulesEngine` consuming `BallState` + `ShotIntent` -> `OnWicketFallen`/`OnOverComplete` `system_design/state_machines_and_event_model.md:33`, Animator not owner `TDD.md:41` presentation cue only `TDD.md:93` contact window `0.40+/-0.04` `TDD.md:93`.
- State `NotStarted->TossPhase->Innings1->Complete` `system_design/state_machines_and_event_model.md:28` + `Delivery` `Bowling->Resolved` `system_design/state_machines_and_event_model.md:34`.

### 5.2 Timing and Outcome

- map batting input to timing quality,
- map bowling release to execution quality,
- map contact quality to ball outcome,
- expose all major outcomes through events.

### 5.3 Control Handoff

- implement AI-to-human fielding transfer,
- preserve movement continuity,
- make handoff explicit and server-friendly for future multiplayer.

### 5.4 State Transitions

- model innings, overs, deliveries, dead ball, and result states,
- make each transition deterministic,
- avoid hidden state changes inside UI or camera code.

## 6. Expected Output

- A full solo match can complete.
- Cricket rules behave consistently.
- Timing, bowling, and fielding all matter.
- The game can be tuned toward believable cricket instead of generic sports behavior.

## 7. Dependencies

- Feature 01 data layer
- Feature 02 animation pipeline
- Feature 03 scene pipeline
- `rules_engine_spec.md`
- `controller_handoff_spec.md`
- `TDD.md`

## 8. References

- [rules_engine_spec.md](../rules_engine_spec.md)
- [controller_handoff_spec.md](../controller_handoff_spec.md)
- [TDD.md](../TDD.md)
- [animation_requirements.md](../animation_requirements.md)
- [development_plan.md](../development_plan.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- create the ball simulation controller,
- create the rules engine,
- create the state machine,
- create scoring and dismissal logic,
- create the handoff logic,
- create tests for ball and rules outcomes,
- create debug readouts for match state.

## 10. Expected Tests

- ball contact tests,
- wicket tests,
- extras tests,
- innings flow tests,
- over progression tests,
- handoff tests,
- AI fill behavior tests.

## 11. Exit Criteria

- A match can start, play, and finish.
- The rules are consistent.
- Fielding, batting, and bowling all work together.
- The game behaves like cricket rather than a prototype.

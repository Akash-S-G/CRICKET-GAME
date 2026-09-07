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

### 3.1 Ball Physics

The ball model must support:

- flight,
- swing,
- seam,
- bounce,
- spin,
- drag,
- contact response,
- pitch reaction.

This should be deterministic enough to test and tune.

### 3.2 Batting

Batting must support:

- late shot intent,
- timing windows,
- shot direction,
- shot type,
- power,
- mistakes and mishits,
- run creation,
- defensive play.

Batting should feel like a decision under pressure, not a button press.

### 3.3 Bowling

Bowling must support:

- delivery type selection,
- release timing,
- line and length,
- accuracy quality,
- pace/spin variation,
- follow-through and recovery.

Bowling should feel controlled and tactical.

### 3.4 Fielding

Fielding must support:

- AI field setup,
- human control handoff,
- pickup,
- stop,
- throw,
- catch,
- dive,
- boundary save,
- relay,
- run-out pressure.

Fielding should remain readable and responsive.

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

### 5.1 Domain Model

- build a pure-ish domain layer for rules and match state,
- keep gameplay outcomes testable,
- separate match resolution from presentation events.

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

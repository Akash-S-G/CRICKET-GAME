# Physics Tick and Reconciliation

## Purpose

This doc defines the simulation tick model and the rules for reconciling client prediction with authoritative state.

## Tick Rate

- Minimum simulation rate: `30 Hz`
- Preferred when bandwidth and device budget permit: `60 Hz` for sensitive interactions

## Simulation Rules

1. Ball physics runs on a fixed tick.
2. Contact resolution must be deterministic for the same inputs.
3. Presentation code may interpolate, but never becomes the source of truth.
4. Gameplay state should only advance on fixed simulation steps.

## Sub-Stepping

Use sub-stepping when the ball is moving fast enough that a single tick would skip critical collision or contact events.

Recommended triggers:

- fast deliveries,
- bat-ball contact windows,
- near-stump edge cases,
- close fielding catches.

## Reconciliation Rules

1. Server-authoritative state wins.
2. Clients may predict local input response for responsiveness.
3. Periodic authoritative snapshots must correct drift.
4. Corrections should blend when the error is visual only and not rule-critical.
5. Rule-critical corrections should snap or replay the affected delivery segment as needed.

## State Ownership

- Ball state: authoritative on server or single simulation host
- Batting input intent: local prediction allowed, server resolution authoritative
- Fielding movement: local prediction allowed for human-controlled slot
- Match result: authoritative only

## Replication Cadence

Transmit:

- compact snapshots each tick group,
- critical events immediately,
- less critical animation or UI hints at reduced frequency.

## Determinism Rules

- use fixed math patterns where practical,
- keep random seeds explicit,
- do not mix presentation timing into physical outcomes,
- keep the same tick order across clients and server.

## Dependencies

- rules engine
- controller handoff
- future multiplayer architecture

## Tests

- identical inputs produce stable outputs
- reconciliation does not corrupt delivery outcomes
- sub-stepping captures fast collision cases

## Exit Criteria

- simulation timing is explicit
- reconciliation behavior is defined before multiplayer code begins
- the physics loop is stable enough for later server authority


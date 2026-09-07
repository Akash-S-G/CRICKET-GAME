# Feature 04.0: Ball State and Timing Table

## Purpose

This doc locks the ball simulation data shape and the key timing windows used by batting and bowling.

## Ball State

The runtime ball state must at minimum include:

- `position`
- `velocity`
- `spin`
- `seamAngle`
- `bounceCount`
- `isInFlight`
- `isDead`
- `lastContactType`
- `currentDeliveryId`
- `ownerSystem`

## Timing Windows

Use the following initial timing defaults:

- batting perfect window: `60 ms`
- batting acceptable window: `120 ms`
- bowling release window: `80 ms`
- fielding handoff window: mode-tunable, start at `150 ms`

These values are starting points and must be tuned in remote config or data assets, not hard-coded in gameplay behavior.

## Outcome Table

Resolution should distinguish:

- clean bat contact
- edge
- miss
- defensive block
- mistimed shot
- bowled contact
- catchable loft
- run-scoring contact

## Physics Update Rules

1. Ball updates on a fixed tick.
2. Collision checks must be deterministic for the same inputs.
3. Sub-stepping is required for fast deliveries.
4. Contact windows must map to animation events.

## Exit Criteria

- the ball struct is concrete enough for implementation
- timing windows are named and visible to tuning
- the game can resolve batting and bowling against the same state


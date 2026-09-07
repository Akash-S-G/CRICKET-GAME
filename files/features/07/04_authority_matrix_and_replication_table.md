# Feature 07.4: Authority Matrix and Replication Table

## Purpose

This is the implementation contract for ownership, replication cadence, and correction behavior. Animation and camera systems consume replicated state; they never become the source of truth.

## Authority Matrix

| System | Authority | Client may send | Client may predict | Server validates |
|---|---|---|---|---|
| Match phase | Server | intent to continue/leave | no | phase transition |
| Ball simulation | Server | delivery intent | local visual preview only | state and tick |
| Bat input | Batter client | shot type, direction, timestamp | local pose/feedback | timing window and legal state |
| Bat contact result | Server | none | no | contact and outcome |
| Score/wicket/extras | Server | none | no | rules engine |
| Fielder movement | Assigned client | movement intent | local movement | bounds and ownership |
| Fielding handoff | Server | request/ack intent | camera transition only | handoff token and expiry |
| Camera | Local client | mode preference | yes | not replicated |
| Animation | Local presentation | none | yes | event timing receives authoritative cues |
| Progression rewards | Server when online, local idempotent service offline | activity completion request | reward preview only | activity ID and result |

## Replication Cadence

| State | Cadence | Reliability | Interpolation |
|---|---:|---|---|
| Ball authoritative snapshot | 30 Hz during delivery | Unreliable sequenced + keyframes | 50 ms buffer |
| Batter/fielder movement | 10 Hz | Unreliable sequenced | 100 ms buffer |
| Delivery start/contact/result | Event-driven | Reliable ordered | none |
| Score, wickets, over state | Event-driven + end-of-ball snapshot | Reliable ordered | none |
| Handoff token | Event-driven | Reliable ordered | 100 ms blend |
| Lobby/session state | Event-driven | Reliable ordered | none |

## Tick and Identity Rules

- Every authoritative gameplay event carries `matchId`, `deliveryId`, `serverTick`, and `eventSequence`.
- `deliveryId` is unique within a match and is never reused after reconnect.
- Clients discard stale sequences and apply a keyframe before accepting deltas after a gap.
- The server accepts a client input only once per `(playerId, deliveryId, inputSequence)`.
- Animation events use authoritative delivery IDs so late packets cannot trigger duplicate sound, VFX, or reward events.

## Handoff Rules

1. Server selects the fielder and issues a handoff token with expiry.
2. Assigned client acknowledges within 250 ms or the server assigns AI control.
3. During the 100 ms camera blend, gameplay authority does not change twice.
4. On disconnect, the server preserves the ball state and transfers only the controllable intent channel.
5. A reconnecting client receives a keyframe before regaining control.

## Acceptance Tests

- A packet gap triggers keyframe recovery rather than cumulative drift.
- Duplicate delivery events do not duplicate score, audio, VFX, or XP.
- Handoff succeeds at 0, 100, and 150 ms one-way delay.
- AI takes control after the handoff timeout.
- Camera mode changes remain local and never affect authoritative state.

## Dependencies

- `system_design/physics_tick_and_reconciliation.md`.
- `controller_handoff_spec.md`.
- Feature 04 ball and rules contracts.
- Feature 05 camera presentation.

## Expected Outputs

- Authority matrix implemented as code ownership boundaries.
- Replicated DTOs with version fields.
- Network simulator tests for delay, loss, duplication, and reordering.
- Handoff debug overlay showing token, owner, tick, and correction count.


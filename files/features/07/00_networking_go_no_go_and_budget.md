# Feature 07.0: Networking Go/No-Go and Budget

## Purpose

This document prevents premature networking implementation. The project first proves the authority and replication model with a small local/LAN slice, then decides whether the selected transport can meet mobile constraints.

## Initial Transport Decision

Use Unity Netcode for GameObjects for the proof slice because the project is Unity-first and already requires an explicit NGO package lock in Feature 00. Do not introduce a second networking framework during the proof.

A transport change requires a written decision record comparing API stability, server support, mobile reliability, bandwidth, and migration cost.

## Go/No-Go Gates

Proceed to broader online implementation only if all are true:

- Two clients complete 20 consecutive deliveries without divergent score, wicket, or innings state.
- A 100 ms one-way delay and 2% packet loss do not stall the match state machine.
- A 150 ms one-way delay keeps the control handoff readable and recoverable.
- The server/host remains authoritative for every final scoring event.
- Sustained bandwidth stays within the budget in the next section.
- Disconnect and reconnect tests complete without duplicating a delivery or reward.

If any gate fails, keep online work in a sandbox branch and fix the authority boundary before adding features.

## Bandwidth Budget

Budget is per connected player, excluding transport overhead where the profiler reports it separately:

| Traffic | Target | Hard ceiling |
|---|---:|---:|
| Gameplay state and events | 12 KB/s average | 24 KB/s |
| Lobby/session messages | 2 KB/s average | 8 KB/s |
| Voice/chat | out of MVP scope | out of MVP scope |
| Burst during delivery resolution | 24 KB/s for <500 ms | 48 KB/s |

Never replicate transforms for every player at render frequency. Use the cadence and interest rules in `07/04_authority_matrix_and_replication_table.md`.

## Proof Match Scope

The proof supports:

- two players,
- one short innings per side,
- one stadium,
- AI backfill for missing fielders,
- no ranked matchmaking,
- no persistent online inventory authority.

## Acceptance Tests

- The profiler records bandwidth and packet loss for every network test.
- A rejected client input cannot directly change score or wicket state.
- Replayed authoritative events produce the same match result.
- A transport failure returns the player to a safe results or offline state.

## Dependencies

- Feature 04 deterministic match state.
- Feature 06 mobile session and offline behavior.
- Feature 00 package/version lock.
- `system_design/physics_tick_and_reconciliation.md`.

## Expected Outputs

- A reproducible two-client proof scene.
- Network test configuration for 0, 50, 100, and 150 ms one-way delay.
- Bandwidth and divergence report.
- Go/no-go decision record.


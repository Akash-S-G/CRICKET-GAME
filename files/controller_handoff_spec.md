# Controller & Handoff Architecture Spec

## 1. Purpose

Defines exactly when and how control of a fielding position transfers between AI and a human player. This is the highest-risk system in the project — get the state machine right on paper before writing netcode against it.

## 2. State Diagram

```
        ┌────────────┐
        │    Idle     │  (no assigned human this match — pure AI slot)
        └─────┬──────┘
              │ (n/a — stays AI-controlled entire match)
              ▼
   ┌─────────────────────┐   ball trajectory intersects   ┌──────────────────────┐
   │   AI-Controlled      │ ──────── handoff radius ─────► │ Handoff-Triggered     │
   │ (human assigned but  │                                 │ (server validates,    │
   │  ball far away)      │ ◄──── handoff-back trigger ──── │  broadcasts RPC)      │
   └─────────────────────┘                                 └──────────┬────────────┘
                                                                        │ confirmed
                                                                        ▼
                                                             ┌──────────────────────┐
                                                             │  Human-Controlled     │
                                                             │ (player has direct    │
                                                             │  input authority)     │
                                                             └──────────┬────────────┘
                                                                        │ action resolved
                                                                        │ (catch/throw/miss)
                                                                        │ OR disconnect
                                                                        ▼
                                                             back to AI-Controlled
```

## 3. Handoff Trigger Conditions

- **Primary trigger:** ball's server-predicted trajectory (computed each tick from current velocity/spin/drag) intersects a configurable radius (`handoff_radius_m`, mode-tunable — smaller for Main Mode's precision, larger for casual modes' forgiveness) around a fielder position occupied by an assigned human.
- **Priority resolution when multiple eligible humans qualify:** the human whose predicted interception time (not just distance) is soonest gets control. Ties broken by whoever is the "designated" fielder for that zone per the pre-ball field-setting assignment.
- **Non-eligible cases:** a human already mid-action (executing a dive/throw from a previous handoff) is not eligible for a second simultaneous handoff — queue or fall through to next-priority player.
- **AI does not "compete"** for control — AI only acts when no human is eligible/available for that slot at that moment.

## 4. Handoff Confirmation Flow (networking)

1. Server detects trigger condition (server-authoritative, per TDD §3).
2. Server sends `HandoffRequestRPC` to the target client, including current `PhysicalState` (position, velocity, current animation state/normalized time).
3. Client's `HumanController` initializes from that exact `PhysicalState` — no snap, no idle-pose reset. Player should feel like they "caught up to" a fielder already in motion.
4. Server broadcasts `HandoffConfirmedRPC` to all clients so the ownership change is visible to everyone (e.g., UI indicator showing which player controls which fielder).
5. On action resolution (ball fielded/thrown/missed) or a fixed timeout (ball has clearly moved away), server issues `HandoffReleaseRPC`, returning the slot to AI with the human's final `PhysicalState` as the AI's continuation point.

## 5. Disconnect / Reconnect Handling

These are distinct cases — do not use one generic "player left" handler:

- **Disconnect mid-delivery (batting/bowling role):** AI must take over batting/bowling decision-making for the *current ball in flight* using the human's last input intent where possible (e.g., if they'd already committed to a shot direction, let AI complete a plausible version of that shot rather than freezing).
- **Disconnect mid-run-chase (fielder actively converging on ball):** AI inherits current velocity/position and continues the intercept — must not visibly stutter or reset.
- **Disconnect while idle (AI-controlled slot with assigned-but-inactive human):** trivial — slot was already effectively AI-driven for positioning; simply mark as unassigned/backfilled for the rest of the match (or until reconnect, if reconnect window is supported).
- **Reconnect:** if within a defined grace window, human reclaims their slot at the next natural break (end of over, wicket, innings break) rather than mid-ball, to avoid another jarring mid-action handoff.

## 6. Latency Compensation

- The moment of handoff is the highest-risk point for visible glitches. Mitigate via:
  - Sending `PhysicalState` slightly ahead (extrapolated by estimated RTT) so the client starts from where the fielder will actually be by the time input starts applying, not where it was `RTT` ago.
  - Short blend/interpolation window (not a hard snap) if there's a small discrepancy between predicted and actual handoff state.
- Test this explicitly at the 2-player-online milestone (see `roadmap.md`) with artificial latency injection (100–150ms) before scaling to 22 players.

## 7. AI Difficulty Scaling

`AIController` behavior should read a difficulty parameter from `ModeConfig.ai_difficulty_default` (and per-slot override if a specific AI player should be harder/easier, e.g. difficulty scaling by match importance):

- **Full (Main Mode):** proper shot selection logic, realistic fielding positioning/anticipation, sensible bowling variation selection.
- **Simplified (Gully/MinBoundary):** more forgiving reaction times, simpler decision trees — matches the casual tone and keeps CPU cost low on mobile.

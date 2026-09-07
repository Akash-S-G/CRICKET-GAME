# Remote Config and Analytics

## Purpose

This doc defines the tuning keys and telemetry events that the game may use in production.

## Remote Config Keys

| Key | Type | Default | Purpose |
|---|---:|---:|---|
| `tutorial_timing_ms` | int | 120 | Delay between tutorial hints |
| `batting_timing_window_ms` | int | 180 | Valid timing window around contact |
| `handoff_radius_m` | float | 3.5 | Fielder handoff trigger radius |
| `physics_tick_hz` | int | 30 | Simulation tick rate |
| `low_device_quality_profile` | string | `low` | Device tier override |
| `progression_xp_multiplier` | float | 1.0 | Reward tuning |
| `analytics_enabled` | bool | true | Global telemetry switch |
| `replay_enabled` | bool | true | Presentation feature flag |

## Remote Config Rules

1. Every key must have a local fallback.
2. Unknown keys should be ignored safely.
3. Config refresh must not block app boot.
4. Cached values should be used when remote fetch fails.
5. Critical gameplay values should be versioned and documented.

## Analytics Provider

Use one analytics provider consistently per environment.

Requirement:

- the provider must support offline queueing,
- batching,
- screen tracking,
- custom event payloads,
- crash/error hooks.

## Required Events

### Session Events

- `session_start`
- `session_end`
- `session_resume`
- `session_fallback`

### Match Events

- `match_start`
- `match_end`
- `innings_start`
- `innings_end`
- `over_start`
- `over_end`
- `delivery_resolved`

### Gameplay Events

- `shot_played`
- `wicket_fallen`
- `bowling_type_selected`
- `camera_mode_changed`
- `handoff_triggered`
- `handoff_confirmed`
- `save_conflict_detected`

### Progression Events

- `reward_granted`
- `unlock_completed`
- `tutorial_completed`

## Payload Rules

- payloads must be small,
- payload keys must be stable,
- no raw personal data,
- no full account identifiers unless required by the provider and approved by privacy policy,
- no unbounded arrays.

## Offline Queue Rules

1. Buffer events locally when network is unavailable.
2. Flush on next successful connection or app-safe checkpoint.
3. Drop stale events after a defined retention window.
4. Do not let analytics failures block gameplay.

## Dependencies

- Feature 01 data and service layer
- save schema versioning
- security and privacy policy

## Tests

- events serialize consistently
- fallback values are used when config is unavailable
- offline queue flushes safely

## Exit Criteria

- tuning and telemetry can be changed without code edits where intended
- event names and payloads are stable
- analytics cannot break the game


# Feature 08.4: Release Gates and Observability

## Purpose

This document defines the measurable release gates and the minimum telemetry needed to detect failures after deployment without collecting unnecessary personal data.

## Release Gates

A release candidate is blocked if any required gate fails:

| Gate | Required result |
|---|---|
| Cold launch | 10 consecutive launches without a crash or blank screen |
| Core flow | Nets, Quick Match, results, save, and resume pass on Low and Mid devices |
| Save safety | 1,000 save/load cycles with zero unrecoverable profiles |
| Schema migration | Every fixture migrates to the current version without data loss |
| Performance | 95% of frames meet the tier target in the five-minute capture |
| Content loading | 100% of Boot and Nets assets load offline from the packaged fallback |
| Offline mode | Nets and Quick Match complete with network disabled |
| Network proof | No authority divergence in the configured two-client test matrix |
| Crash-free sessions | At least 99% in the release-candidate soak sample |

## Observability Provider and Policy

Use Unity Cloud Diagnostics for crash and exception reporting, with the analytics schema from `system_design/analytics_events_schema.json`. If the provider is unavailable, local diagnostics remain available and gameplay continues.

Do not send display names, email addresses, raw device identifiers, or free-form user text in gameplay analytics. Use an installation-scoped pseudonymous ID and the documented consent/settings state.

## Critical Events

The following events are mandatory for release diagnostics:

- `app_boot_started`, `app_boot_completed`, `app_boot_failed`
- `scene_load_started`, `scene_load_completed`, `scene_load_failed`
- `save_write_completed`, `save_write_failed`, `save_recovered`
- `session_started`, `session_resumed`, `session_abandoned`, `session_completed`
- `content_load_failed`
- `network_session_started`, `network_session_ended`, `network_reconnect_failed`
- `handoff_started`, `handoff_completed`, `handoff_timeout`
- `performance_tier_changed`

Events must include schema version, app version, platform, quality tier, session ID, and a correlation ID. They must exclude raw payloads that can contain personal information.

## Alert Thresholds

- `app_boot_failed` above 1% of sessions: investigate before rollout expansion.
- `save_write_failed` above 0.1%: pause progression-affecting rollout.
- `content_load_failed` above 0.5%: disable affected remote content.
- `handoff_timeout` above 5% in network proof: block multiplayer expansion.
- crash-free sessions below 99%: halt release rollout.

## Rollback and Kill Switches

Remote config must be able to disable:

- remote cosmetic content,
- online lobby entry,
- experimental camera modes,
- experimental progression rewards.

Core offline Nets and Quick Match must remain available when optional services are disabled.

## Acceptance Tests

- Every critical event validates against the analytics schema.
- Provider failure does not block app boot or match completion.
- A remote kill switch disables an optional feature within one fetch interval.
- Release reports contain build version and commit SHA.
- A crash report can be correlated to the last scene and session phase without exposing user identity.

## Dependencies

- `system_design/observability_stack.md`.
- `system_design/remote_config_and_analytics.md`.
- `system_design/security_threat_model.md`.
- Feature 06 offline behavior.

## Expected Outputs

- Release checklist encoded as CI and manual gates.
- Crash/analytics initialization with offline-safe fallback.
- Dashboard queries or saved reports for the thresholds above.
- Documented rollback procedure.


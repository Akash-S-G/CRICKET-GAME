# Feature 06.0: Mobile MVP Contract

## Purpose

This document locks the measurable behavior of the first mobile release. It is the contract for the short-session loop, offline behavior, save/resume checkpoints, and mobile-first navigation.

An agent must not invent a different session length, menu route, or offline policy while implementing Feature 06.

## Release Scope

The first mobile MVP contains:

- Nets practice.
- Quick Match with one short innings per side.
- Local profile progression.
- Offline play for Nets and Quick Match.
- Save and resume after app backgrounding or an interrupted match.
- Low, Mid, and High quality presets.

The MVP does not contain ranked online multiplayer, purchases, live events, or a server-dependent login requirement.

## Session Targets

| Flow | Target | Maximum allowed |
|---|---:|---:|
| Cold launch to Home on a Mid device | 8 seconds | 12 seconds |
| Home to Nets gameplay | 2 taps | 4 taps |
| Home to Quick Match gameplay | 3 taps | 5 taps |
| Nets practice session | 60-120 seconds | 3 minutes |
| Quick Match session | 2-5 minutes | 8 minutes |
| Resume after backgrounding | 3 seconds | 6 seconds |
| Results to next playable session | 2 taps | 4 taps |

The timings are measured from the first visible frame or user tap. Loading screens must show progress and must not leave the player on an unresponsive blank screen.

## Session State Contract

`SessionCheckpoint` must contain:

```text
schemaVersion: int
sessionId: string
modeId: string
sceneId: string
phase: enum { Setup, Delivery, Contact, Resolution, Results }
inningIndex: int
overIndex: int
ballIndex: int
score: int
wickets: int
target: int?
activeBatterId: string
activeBowlerId: string
lastTrustedEventId: string
createdAtUtc: string
updatedAtUtc: string
```

Only a checkpoint at a safe phase may be resumed. If the app closes during `Contact` or `Resolution`, restore from the previous trusted delivery boundary and show a short recovery notice.

## Offline Rules

- Nets and Quick Match remain playable without network access.
- Profile, progression, and settings write to the local save service.
- Online-only operations are hidden or disabled, never shown as silently failing buttons.
- Analytics events enter the offline queue defined by `system_design/remote_config_and_analytics.md`.
- A save conflict is resolved by the Feature 01 policy; Feature 06 must not create a second merge algorithm.

## Acceptance Tests

1. A new install reaches Home without network access.
2. A player starts Nets in four taps or fewer.
3. A player completes three deliveries in under two minutes.
4. Backgrounding during setup resumes the same session.
5. Force-closing during delivery restores the previous trusted delivery boundary.
6. Completing a Quick Match awards results once, even if the results screen is reopened.
7. Switching quality presets does not reset match or profile state.

## Dependencies

- Feature 01 save, profile, session, and analytics services.
- Feature 03 scene and navigation pipeline.
- Feature 04 match and delivery state.
- Feature 05 input and tutorial flow.
- `system_design/save_schema_versioning.md`.

## Expected Outputs

- `MobileSessionService` integration with the shared service registry.
- `SessionCheckpoint` serialization and recovery tests.
- Nets and Quick Match entry routes.
- Offline banners and disabled online affordances.
- A device test report containing the timing targets above.


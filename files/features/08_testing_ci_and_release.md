# Feature 08: Testing, CI, and Release Hardening

## 1. What This Feature Is

This feature makes the project production-ready.

It covers:

- tests,
- CI,
- build validation,
- mobile validation,
- multiplayer validation,
- crash and telemetry readiness,
- release-safe fallback behavior.

## 2. Why This Feature Matters

The game is large enough that manual testing alone will not be enough.

This feature protects the project from:

- regressions,
- broken scenes,
- broken saves,
- broken mobile performance,
- broken multiplayer assumptions,
- unsafe release changes.

## 3. Test Strategy

Detailed contracts:

- [CI toolchain and commands](08/00_ci_toolchain_and_commands.md)
- [Release gates and observability](08/04_release_gates_and_observability.md)

### 3.1 Edit-Mode Tests (`Unity -runTests -testMode EditMode` `system_design/build_pipeline.md:16` + GameCI `system_design/build_pipeline.md:8`)

- `ajv` validate `mode_config_schema.json:2`/`player_schema.json:2`/`ground_schema.json:2` `system_design/build_pipeline.md:16` CI.
- `ISaveService` round-trip + migration `ProfileDataV2->V3` `system_design/save_schema_versioning.md:45` + quarantine `system_design/save_schema_versioning.md:38`.
- Rules pure `features/04/04_rules_engine_and_match_state.md:16` `OnWicketFallen` + `system_design/state_machines_and_event_model.md:33` deterministic `system_design/physics_tick_and_reconciliation.md:24`.

### 3.2 Play-Mode Tests

Use play-mode tests for runtime flows:

- scene transitions,
- gameplay loops,
- camera switching,
- tutorial flow,
- HUD behavior,
- replay behavior.

### 3.3 Mobile Tests

Test:

- frame rate,
- loading time,
- battery impact,
- UI scale,
- save/resume,
- low-end performance tiers.

### 3.4 Multiplayer Tests

Test:

- latency,
- state sync,
- handoff behavior,
- reconnect behavior,
- AI backfill,
- authority consistency.

## 4. CI Strategy

### 4.1 Build Checks (GitHub Actions + GameCI `system_design/build_pipeline.md:8` fail-fast `system_design/build_pipeline.md:34`)

- Restore packages, open project, `EditMode` + `PlayMode` `system_design/build_pipeline.md:16` steps 1-7, build Android + dedServer `system_design/build_pipeline.md:16`.
- Schema `ajv`, asset refs, `Tests/EditMode`/`PlayMode` `unity_ai_workflow_and_project_structure.md:85` + `features/08/01_edit_and_play_mode_tests.md:16`.
- Artifacts logs+build_hash+tag `system_design/build_pipeline.md:34`; main protected `system_design/build_pipeline.md:34`.

### 4.2 Release Validation

Add checks for:

- crash reports,
- analytics event integrity,
- remote config fallback,
- offline fallback,
- content load reliability.

## 5. Production Hardening

### 5.1 Fallbacks

The game must fail safely:

- offline if services fail,
- cached data if remote data is missing,
- local play if online play is unavailable,
- default tuning if remote config is absent.

### 5.2 Observability (Crash + sampling `system_design/observability_stack.md:16`)

- UGS Diagnostics + `system_design/observability_stack.md:24` breadcrumbs, `session_id/profile_id/build_hash` `system_design/analytics_events_schema.json:1` per `IAnalyticsService` `system_design/service_interfaces.md:34` with queue `system_design/remote_config_and_analytics.md:39`.
- Metrics `boot time/scene load/save failures/config fetch/match abandon/reconnect` `system_design/observability_stack.md:24` + `handoff_triggered/confirmed` `system_design/remote_config_and_analytics.md:34` sampled but failures never sampled `system_design/observability_stack.md:24`.

## 6. Expected Output

- The game can be built and checked automatically.
- Regressions are caught early.
- Release confidence is high.
- Failures degrade gracefully instead of breaking the experience.

## 7. Dependencies

- All earlier features
- `milestone_checklist.md`
- `risk_resolution.md`
- `system_design.md`

## 8. References

- [milestone_checklist.md](../milestone_checklist.md)
- [risk_resolution.md](../risk_resolution.md)
- [system_design.md](../system_design.md)
- [development_plan.md](../development_plan.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- create test scaffolds,
- create build scripts,
- create validation scripts,
- create CI helper scripts,
- create runtime debug outputs,
- create fallback handling,
- create telemetry checks.

## 10. Exit Criteria

- Required CI jobs pass from a clean runner using the pinned Unity version.
- EditMode, PlayMode, schema, scene, and Android development-build checks run automatically.
- Save/load survives 1,000 cycles with zero unrecoverable profiles.
- Offline Nets and Quick Match remain playable when optional services fail.
- Release candidate meets the documented performance, content-loading, network-proof, and crash-free-session gates.

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

### 3.1 Edit-Mode Tests

Use edit-mode tests for logic that does not need the scene to run:

- schema validation,
- config loading,
- save/load logic,
- rules logic,
- state transitions,
- deterministic helpers.

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

### 4.1 Build Checks

Automate:

- compile validation,
- scene load checks,
- schema validation,
- content checks,
- test execution.

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

### 5.2 Observability

Track:

- crashes,
- startup failures,
- scene load failures,
- save corruption,
- network issues,
- handoff failures,
- player dropoff points.

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

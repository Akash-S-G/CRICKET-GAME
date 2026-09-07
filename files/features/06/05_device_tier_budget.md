# Feature 06.5: Device Tier Budget

## Purpose

This document converts the general mobile quality tiers into enforceable budgets. Runtime quality selection must use these profiles as the defaults and may only lower quality automatically, never raise it above the detected device budget.

## Tier Definitions

| Budget | Target device class | FPS target | Resolution scale | Draw calls | Visible triangles | Texture memory |
|---|---|---:|---:|---:|---:|---:|
| Low | Adreno 610 class, 2 GB RAM | 30 stable | 0.70 | 60 | 1.5M | 160 MB |
| Mid | Adreno 730 class, 4-6 GB RAM | 45 stable, 60 preferred | 0.85 | 90 | 2.5M | 256 MB |
| High | Recent flagship, 8 GB+ RAM | 60 stable | 1.00 | 120 | 4.0M | 384 MB |

The FPS target is evaluated over a five-minute match capture. A stable target means at least 95% of sampled frames meet the target and no continuous stall exceeds 250 ms.

## Rendering Presets

| Setting | Low | Mid | High |
|---|---|---|---|
| Shadows | Off for crowd, 512 px main light | 1024 px main light | 2048 px main light |
| Crowd | 25% impostors | 60% simplified characters | 100% LOD characters |
| Post-processing | Color grading only | Color grading + bloom | Full approved stack |
| Anti-aliasing | FXAA/off | 2x MSAA or approved mobile AA | 4x MSAA where supported |
| Hair/cloth | Baked or disabled | simplified simulation | full approved simulation |
| Replay quality | same as gameplay | same as gameplay | enhanced if budget allows |

## Automatic Fallbacks

1. Start with the stored user setting if it is valid for the device.
2. If the first 10 seconds exceed the frame-time budget by 20%, lower one preset level.
3. If memory warning or thermal warning is received, disable crowd detail and replay enhancements first.
4. Persist the fallback and show a non-blocking notice in Settings.
5. Never change input sensitivity, timing windows, or simulation constants as a performance fallback.

## Acceptance Tests

- Each tier loads its intended URP and quality settings.
- A five-minute capture meets the FPS and frame-time thresholds on one representative device.
- Low tier remains readable at the minimum supported phone resolution.
- Quality changes apply without corrupting the active session.
- A memory warning reduces presentation load without changing match results.

## Dependencies

- `system_design/addressables_grouping.md`.
- `system_design/ui_architecture_and_navigation.md`.
- Feature 03 UI scale rules.
- Feature 08 mobile performance test harness.

## Expected Outputs

- Three named quality profiles.
- Runtime `DeviceTierService` and fallback telemetry.
- Per-tier capture results stored with build and device metadata.


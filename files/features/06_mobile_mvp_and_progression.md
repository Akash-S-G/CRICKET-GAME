# Feature 06: Mobile MVP and Progression

## 1. What This Feature Is

This feature turns the game into a real mobile product.

It includes:

- touch-first play
- offline support
- quick sessions
- save/resume
- progression
- rewards
- mobile UI polish
- session-friendly retention

## 2. Why This Feature Matters

This game is mobile-first, so this feature is not optional polish.

It must make the game:

- easy to start,
- easy to return to,
- easy to understand,
- good in short bursts,
- stable on a phone,
- rewarding without being unfair.

## 3. Mobile MVP Design

Detailed contracts:

- [Mobile MVP contract](06/00_mobile_mvp_contract.md)
- [Progression numbers and unlock table](06/04_progression_numbers_and_unlock_table.md)
- [Device tier budget](06/05_device_tier_budget.md)

### 3.1 Entry Points

The two most important entry points are:

- Quick Match
- Nets

The mobile game should launch into one of those quickly.

### 3.2 Session Behavior

The game should support:

- short matches,
- interrupted play,
- resume later,
- offline practice,
- quick navigation,
- low friction after opening the app.

### 3.3 UI Behavior

The UI must:

- remain readable on smaller screens,
- keep tap targets large enough,
- avoid clutter,
- show important match information clearly,
- support one-handed or low-friction usage where practical.

## 4. Progression Design

### 4.1 What Progression Does (non-P2W `GDD.md:214`, validated server-side `system_design/security_threat_model.md:16`)

- XP per match `GDD.md:214` + local `profile.json` progression `system_design/save_schema_versioning.md:33` synced field-merge `system_design/save_schema_versioning.md:46`; server validates `OnRewardGranted` `system_design/state_machines_and_event_model.md:33` via Cloud Code `system_design/remote_config_and_analytics.md:16` `progression_xp_multiplier:1.0` default.
- Tracked via `reward_granted`/`unlock_completed` `system_design/remote_config_and_analytics.md:34` with `session_id/profile_id/build_hash` `system_design/analytics_events_schema.json:1`.

### 4.2 What Progression Unlocks

Progression can unlock:

- cosmetics,
- profile badges,
- camera skins or overlays,
- practice content,
- challenge content,
- squad customization,
- special events,
- career milestones.

### 4.3 What Progression Must Not Do

Progression must not:

- break fair play,
- make competitive modes pay-to-win,
- require long grind sessions for basic enjoyment.

### 4.4 Mobile Retention Design

The mobile version should have:

- daily missions,
- quick reward loops,
- short event ladders,
- return-player rewards,
- clear “play again” prompts,
- progress that works in tiny sessions.

## 5. Mobile Performance Tasks

### 5.1 Performance Tiers (locked `system_design.md:188` + `system_design/addressables_grouping.md:31`)

- Low Snapdragon 660 30fps 1.5M tris 70 draws `system_design.md:188` Priority-1 anim `animation_clip_inventory.md:345` no keeper `TDD.md:87` `Boot` 15MB.
- Mid Snapdragon 720G 45-60fps 2.5M 100 draws.
- High Snapdragon 865+ 60fps 3.5M 130 draws.
- Auto-select via `SystemInfo` `features/06/03_mobile_performance_and_settings.md:16` override in Customize `scene_by_scene_setup.md:177`.

### 5.2 Performance Work (`system_design/build_pipeline.md:16` CI perf gate + `system_design/observability_stack.md:24` metrics)

- URP Low/Mid/High `system_design.md:188` + Addressables `CoreGameplay` local `system_design/addressables_grouping.md:16` + additive `scene_by_scene_setup.md:212` `Boot->Home`.
- Metrics `boot time`, `scene load time`, `frame budget violations` `system_design/observability_stack.md:24` sampled; CI builds Android on every `feat/06` push `system_design/build_pipeline.md:8`.
- Battery: Low tier 30fps, no soft shadows, crowd sprites `system_design.md:188`.

### 5.3 Mobile Settings (persisted `ISaveService` `system_design/service_interfaces.md:16`)

- Sensitivity 0.5-2.0 `features/05_camera_input_tutorial_flow.md:87`, `handoff_radius_m` `system_design/remote_config_and_analytics.md:8` fallback 3.5, `CameraConfig` SO `unity_ai_workflow_and_project_structure.md:73`, tier auto + manual `scene_by_scene_setup.md:177` saved in `profile.json` `system_design/save_schema_versioning.md:33` + `TutorialStepData` `player_onboarding_and_tutorial_flow.md:59`.
- Offline-first: settings available without `IRemoteConfigService` fetch `system_design/remote_config_and_analytics.md:16` cached fallback.

## 6. Expected Output

- The game is a usable mobile product.
- Players can return in short bursts.
- Progression feels meaningful without breaking fairness.
- The app is comfortable on a phone.

## 7. Dependencies

- Feature 01 data layer
- Feature 03 scene pipeline
- Feature 05 camera/input/tutorial
- `mobile_roadmap.md`
- `system_design.md`
- `game_flow_and_camera_design.md`

## 8. References

- [mobile_roadmap.md](../mobile_roadmap.md)
- [system_design.md](../system_design.md)
- [game_flow_and_camera_design.md](../game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](../player_onboarding_and_tutorial_flow.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- build quick-match flow,
- build Nets-first mobile flow,
- build save/resume support,
- build progression reward flow,
- build mobile UI simplification,
- build performance tier toggles,
- build return-player UI behavior.

## 10. Expected Tests

- short match completion tests,
- offline play tests,
- save/resume tests,
- progression unlock tests,
- UI scaling tests,
- mobile performance tests.

## 11. Exit Criteria

- Nets starts in four taps or fewer and supports three deliveries in under two minutes.
- Quick Match completes in 2-5 minutes on the Mid target device.
- Nets and Quick Match complete with network disabled.
- Backgrounding and force-close recovery preserve the previous trusted delivery boundary.
- A five-minute capture meets the selected tier's FPS target for at least 95% of frames.
- XP and unlocks are idempotent, persist after restart, and do not alter gameplay constants.

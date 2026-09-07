# Feature 00: Foundation and Tooling

## 1. What This Feature Is

This feature sets up the project foundation:

- Unity project structure
- AI tooling
- MCP tooling
- git workflow
- base services
- bootstrap scene
- package selection

This is the feature that makes the rest of the game possible.

## 2. How To Implement It

### 2.1 Project Setup (locked per `system_design.md:443` + `TDD.md:5`)

- Create Unity 6 LTS `6000.0.x` (pin patch in `ProjectSettings/ProjectVersion.txt` before first commit).
- Configure URP 17.x: create 3 assets `URP-Low`/`URP-Mid`/`URP-High` per `system_design.md:188` tier budgets (shadows off / 512 / 1024).
- Install and pin in `Packages/manifest.json`: `com.unity.inputsystem 1.11.x`, `com.unity.netcode.gameobjects 2.4.x`, `com.unity.addressables 2.x`, `com.unity.animation.rigging 1.2.x`, `com.unity.cinemachine 3.x`, `com.unity.services.*` (Auth/Cloud Save/Remote Config/Analytics), `com.unity.ugui` (runtime uGUI `system_design/ui_architecture_and_navigation.md:8`).
- Apply authoritative folder `Assets/_Project/...` `unity_ai_workflow_and_project_structure.md:18` (over `TDD.md:105` legacy alias) - Scripts/Core/Domain/Gameplay/Services/Animation/Cameras/Input/Save/Analytics/Tools, `ScriptableObjects/Modes/Players/Pitches/Grounds/Cameras`, `Tests/EditMode/PlayMode`.
- Set Git LFS `system_design/build_pipeline.md:34`: `*.png *.psd *.fbx *.wav *.mp3 *.unity *.prefab *.anim *.controller *.bundle filter=lfs`.
- Add `ProjectSettings/ProjectVersion.txt` + `Packages/manifest.json` to first `v0.2-project-setup` tag `feature_implementation_master.md:40`.

### 2.2 Tooling Setup

- Connect Unity MCP.
- Connect the preferred AI code agent.
- Define the repo docs as the source of truth.
- Set up a branch-per-feature workflow.

### 2.3 Bootstrap Systems (`unity_ai_workflow_and_project_structure.md:517` `GameBootstrapper`/`ServiceRegistry`/`SessionController`)

- Create persistent `Boot` `ServiceBootstrap` `DontDestroyOnLoad` holding `GameBootstrapper` -> registers `ISaveService`/`IAuthService`/`IRemoteConfigService`/`IAnalyticsService`/`ISessionService`/`IContentService` `system_design/service_interfaces.md:16` at composition root `system_design/service_interfaces.md:51`.
- `Boot->Auth->Home` state machine `system_design/state_machines_and_event_model.md:16` with `OfflineFallback` on `IAuthService.RestoreSessionAsync` failure.
- Loading screen shows `Progress / 0..1` from `IContentService.PreloadBootContentAsync` `system_design/service_interfaces.md:42` Addressables `Boot` group `system_design/addressables_grouping.md:16`.
- Version init: read `Application.version` + `build_hash` into `Analytics` payload `system_design/analytics_events_schema.json:1`, refresh Remote Config `system_design/remote_config_and_analytics.md:16` non-blocking with fallback `handoff_radius_m: 3.5`.

### 2.3.1 Folder Lock Check

Fail CI if any script appears outside `Assets/_Project/Scripts/...` `system_design/build_pipeline.md:16` branch rule.

### 2.4 Validation Tools (CI-ready per `system_design/build_pipeline.md:16`)

- Structured logs `system_design/observability_stack.md:16` (sessionId/profileId/buildHash) + Crash breadcrumb `system_design/observability_stack.md:24`.
- Boot validation: check `profile.json` schema version, `*.backup.json` fallback `system_design/save_schema_versioning.md:16`.
- Scene-load validation: `Boot`+`Home_Menu` additive load test `scene_by_scene_setup.md:212`, `UIRoot` persistent `system_design/ui_architecture_and_navigation.md:16`.
- Test scaffolds `Tests/EditMode` + `PlayMode` `unity_ai_workflow_and_project_structure.md:85` with `Unity -runTests -testMode EditMode` `features/08_testing_ci_and_release.md:16`. GitHub Actions + GameCI `system_design/build_pipeline.md:8`.

### 2.5 Greenlight Gate (must pass before merge)

Do not merge `feat/00-foundation` until `system_design/service_interfaces.md:60`, `system_design/input_action_maps.md:6`, `system_design/build_pipeline.md:16` exist and compile in isolation `system_design/service_interfaces.md:66`.

## 3. Expected Output

- A working Unity project that opens cleanly.
- A clean repo structure.
- AI tools connected and usable.
- A stable bootstrap flow into the first menu scene.

## 4. Dependencies

- Unity 6 LTS
- URP
- Git
- Git LFS
- Unity MCP
- AI coding tool of choice
- `system_design.md`
- `unity_ai_workflow_and_project_structure.md`

## 5. References

- [TECH_STACK.md](../TECH_STACK.md)
- [system_design.md](../system_design.md)
- [unity_ai_workflow_and_project_structure.md](../unity_ai_workflow_and_project_structure.md)
- [development_plan.md](../development_plan.md)
- [README.md](../../README.md)

## 6. Exit Criteria

- The project boots.
- The project structure is in place.
- The AI workflow is usable.
- The repo is ready for feature work.

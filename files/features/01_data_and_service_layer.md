# Feature 01: Data and Service Layer

## 1. What This Feature Is

This feature builds the reusable data and service backbone for the whole game. Every other system depends on this layer because it determines how the game is configured, saved, synced, tuned, and resumed.

The goal is not just to store data. The goal is to make the game data-driven so that modes, players, cameras, grounds, pitches, and progression can be changed without rewriting gameplay code.

This feature includes:

- mode configs
- player profiles
- pitch data
- ground data
- camera configs
- progression data
- save/load
- remote config
- analytics
- session state
- local cache and sync rules

## 2. Why This Feature Exists

If this layer is weak, every later feature becomes harder:

- scenes will hardcode values,
- gameplay code will own configuration,
- progression will be difficult to balance,
- mobile tuning will be expensive,
- multiplayer later will be brittle,
- AI agents will produce noisy, inconsistent code.

This feature creates the contract that all future features use.

## 3. Data Architecture

### 3.1 ScriptableObject Layer

Use ScriptableObjects for content that is authored in Unity and inspected by designers or AI-assisted workflows.

Examples:

- `ModeConfig`
- `PlayerProfileData`
- `PitchData`
- `GroundData`
- `CameraConfig`
- `ProgressionRewardData`
- `TutorialStepData`

ScriptableObjects should be:

- human-readable,
- editor-friendly,
- easy to reference in scenes and prefabs,
- backed by the schemas in `files/`.

### 3.2 JSON Schema Layer

Use JSON schemas as the source-of-truth format for:

- validation,
- external editing,
- automation,
- import/export,
- versioning,
- AI-friendly generation.

This is the format AI agents can inspect and generate safely.

### 3.3 Runtime Data Layer

At runtime, the game should convert authored data into usable structures:

- loaded config objects,
- cached session values,
- player save state,
- tuned gameplay constants,
- remote overrides.

## 4. Service Design

### 4.1 Save Service

The save service must:

- persist profile data,
- persist progression,
- persist settings,
- store last selected mode,
- store camera preferences,
- cache offline progress,
- sync safely when online data is available.

The service should support:

- local save,
- cloud save,
- merge or overwrite rules,
- safe fallback when the network is unavailable.

### 4.2 Profile Service

The profile service must:

- create profiles,
- select profiles,
- load profiles,
- update profile settings,
- track tutorial completion,
- track unlocks,
- track last played mode.

### 4.3 Session Controller

The session controller must:

- define the current app state,
- coordinate boot to home to match transitions,
- load the correct data for the current mode,
- tell the game whether the session is offline or online,
- preserve temporary state through scene loads.

### 4.4 Remote Config Service

The remote config service must:

- fetch tunable values,
- apply live tuning safely,
- keep defaults in local data,
- fall back when service calls fail.

Use this for:

- difficulty tuning,
- progression tuning,
- camera defaults,
- tutorial tuning,
- event flags,
- performance settings.

### 4.5 Analytics Service

The analytics service must log:

- first launch,
- tutorial completion,
- mode selection,
- match start,
- match finish,
- quit points,
- session length,
- replay usage,
- camera selection,
- difficulty selection.

These events are essential for improving the mobile product.

## 5. Implementation Tasks

### 5.1 Define the Data Contracts (locked per `system_design/save_schema_versioning.md:33` + `features/01/01_data_contracts_and_schemas.md:16`)

- Finalize schemas: `mode_config_schema.json:2` `player_schema.json:2` `pitch_schema.json:2` `ground_schema.json:2` `delivery_physics_constants.json:2` + new `CameraConfig`/`ProgressionRewardData` with `schemaVersion`/`dataVersion`/`updatedAtUtc`/`sourceAppVersion` `system_design/save_schema_versioning.md:33`.
- Required vs optional: `id`/`display_name` required, `rule_overrides` optional with fallback `mode_config_schema.json:18`. Generate `ajv` validation `system_design/build_pipeline.md:16` on CI.
- Migration: `ProfileDataV2->V3` pure `system_design/save_schema_versioning.md:45`, quarantine bad file + restore `profile.backup.json` `system_design/save_schema_versioning.md:38`.
- Authoring: SO `ScriptableObjects/Modes/...` `unity_ai_workflow_and_project_structure.md:73` backs JSON, importer `01/01_data_contracts_and_schemas.md:16` conversion `SO <-> DTO`.

### 5.2 Build Data Loading

- Load authored assets on boot or scene entry.
- Validate loaded data before use.
- Convert external values into runtime-safe structures.
- Cache frequently used data.

### 5.3 Build Save and Sync (Interfaces `system_design/service_interfaces.md:16`)

- Implement `ISaveService` `LoadProfile`/`SaveProfile`/`BackupProfile` atomic temp-file `system_design/save_schema_versioning.md:51` -> `profiles/<profileId>/profile.json`.
- `ISessionService` `CreateSession`/`ResumeSession`/`EndSession` holds `SessionContext` (modeId, sessionId, offline/online flag `system_design/state_machines_and_event_model.md:16`).
- Sync: UGS Cloud Save, field-level merge `system_design/save_schema_versioning.md:46` server wins progression, local wins settings if newer `updatedAtUtc`.
- Failure: corrupted `profile.json` -> quarantine -> `profile.backup.json` -> fresh defaults `system_design/save_schema_versioning.md:38`, emit `save_conflict_detected` `system_design/remote_config_and_analytics.md:34`.

### 5.4 Build Session and Tuning Support (`IRemoteConfigService`/`IAnalyticsService`/`IContentService`)

- `SessionController` persistent `Boot/ServiceBootstrap` `unity_ai_workflow_and_project_structure.md:185` survives scene loads `system_design/ui_architecture_and_navigation.md:16`.
- Load `ModeConfig` SO `features/03_scene_and_ui_pipeline.md:28` before `Match_Setup`, override `batting_timing_window_ms:180` `handoff_radius_m:3.5` via `IRemoteConfigService.GetValue` fallback `system_design/remote_config_and_analytics.md:16`.
- `IAnalyticsService.TrackEvent` `system_design/service_interfaces.md:34` with payload schema `system_design/analytics_events_schema.json:1` (`session_start`,`match_start`,`delivery_resolved`,`handoff_triggered`) + offline queue `system_design/remote_config_and_analytics.md:39`.
- `IContentService` `PreloadBootContentAsync`/`LoadModeContentAsync` `system_design/service_interfaces.md:42` bounds `Boot` 15MB LRU cache `system_design/addressables_grouping.md:31`.
- Temp match state (ball, over) never written to `profiles/`; summary stats persisted on `OnInningsEnded` `system_design/state_machines_and_event_model.md:33`.

## 6. Expected Output

The result of this feature should be:

- a stable data model for the game,
- reusable services for save, config, analytics, and session flow,
- clean separation between authored data and gameplay code,
- reliable persistence across sessions,
- a foundation that later gameplay systems can depend on.

## 7. Dependencies

- Feature 00 foundation
- `system_design.md`
- `TDD.md`
- `game_flow_and_camera_design.md`
- data schemas in `files/`

## 8. References (locked stack)

- [system_design.md](../system_design.md) + subdocs `system_design/INDEX.md:5` read order 1-5
- [system_design/service_interfaces.md](../system_design/service_interfaces.md) `ISaveService`/`IRemoteConfigService`/`IAnalyticsService`
- [system_design/save_schema_versioning.md](../system_design/save_schema_versioning.md) file layout + migration
- [system_design/remote_config_and_analytics.md](../system_design/remote_config_and_analytics.md) keys + events
- [system_design/addressables_grouping.md](../system_design/addressables_grouping.md) groups
- [TECH_STACK.md](../TECH_STACK.md) + `TDD.md:5` Unity 6 + URP + NGO 2.4.x
- [mode_config_schema.json](../mode_config_schema.json) `handoff_radius_m`
- [delivery_physics_constants.json](../delivery_physics_constants.json) timing 60/120ms

## 9. AI Agent Tasks

An AI agent working on this feature should:

- create the ScriptableObject classes,
- create the JSON import/export helpers,
- create the save service,
- create the profile service,
- create the session controller,
- create the remote config integration,
- create the analytics hooks,
- create validation scripts for schemas and save data.

## 10. Expected Tests

- schema validation tests,
- save/load round-trip tests,
- profile persistence tests,
- remote config fallback tests,
- analytics event trigger tests,
- session state tests.

## 11. Exit Criteria

- Save and load work reliably.
- Data loads from authored sources.
- Services are stable and reusable.
- Offline and online paths can share the same model.
- The project can support later gameplay systems without rework.

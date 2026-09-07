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

### 5.1 Define the Data Contracts

- Finalize the schema for modes, players, grounds, pitches, and cameras.
- Define required fields versus optional fields.
- Define validation rules.
- Define version fields so future changes do not break old data.

### 5.2 Build Data Loading

- Load authored assets on boot or scene entry.
- Validate loaded data before use.
- Convert external values into runtime-safe structures.
- Cache frequently used data.

### 5.3 Build Save and Sync

- Save profile and progression state.
- Save settings and camera preferences.
- Save offline progress safely.
- Sync cloud state when online.
- Avoid overwriting a newer trusted state with older local data.

### 5.4 Build Session and Tuning Support

- Make the game session object available to all scenes.
- Load mode data before match start.
- Allow remote config to override local defaults.
- Keep temporary match state separate from permanent profile state.

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

## 8. References

- [system_design.md](../system_design.md)
- [TECH_STACK.md](../TECH_STACK.md)
- [mode_config_schema.json](../mode_config_schema.json)
- [player_schema.json](../player_schema.json)
- [ground_schema.json](../ground_schema.json)
- [pitch_schema.json](../pitch_schema.json)
- [delivery_physics_constants.json](../delivery_physics_constants.json)
- [development_plan.md](../development_plan.md)

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

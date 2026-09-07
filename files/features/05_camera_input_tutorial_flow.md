# Feature 05: Camera, Input, and Tutorial Flow

## 1. What This Feature Is

This feature implements the usability layer of the game.

It includes:

- camera modes,
- touch input mapping,
- controller input mapping,
- onboarding,
- tutorial flow,
- first-run learning,
- return-player guidance,
- accessibility and assist settings.

## 2. Why This Feature Matters

This feature determines whether the player can actually understand and enjoy the game.

Even if the simulation is good, the game will fail if:

- the player cannot read the action,
- the inputs are confusing,
- the tutorial is too long,
- the camera is wrong,
- the player cannot recover after returning to the game later.

## 3. Camera System

### 3.1 Required Camera Modes

Implement:

- `FPP / Batter View`
- `Batting Broadcast Camera`
- `Bowling End Camera`
- `Mid-Wicket Tactical Camera`
- `Chase Camera`
- `Top-Down Tactical Camera`
- `Replay Camera`

### 3.2 Camera Behavior (locked: Cinemachine 3.x `TDD.md:8`, blend 0.3s `system_design/physics_tick_and_reconciliation.md:16`)

- Defaults per `scene_by_scene_setup.md:316` `Nets` FPP/Batting Broadcast/Bowling End/TopDown, `Match_Play` 5 modes `game_flow_and_camera_design.md:272` FOV 60 batting /70 top-down; per-mode lock in `CameraConfig` SO `ScriptableObjects/Cameras/` `unity_ai_workflow_and_project_structure.md:73`.
- Switch <0.3s, never block `system_design/input_action_maps.md:31` Input Routing 2 (gameplay) vs 3 (camera).
- URP tier: Low 30fps no post `system_design.md:188`.

### 3.3 Camera Tasks

- build camera presets,
- build camera switching,
- build camera defaults per mode,
- build camera UI for settings,
- make camera transitions smooth,
- make replay cameras suitable for highlights.

## 4. Input System

### 4.1 Touch Controls (Action Maps `system_design/input_action_maps.md:16` + thresholds `system_design/input_action_maps.md:31`)

- Maps: Batting `AimDirection`/`ShotIntent`/`ShotPower`/`TapTiming`, Bowling `LineAim`/`LengthAim`/`ReleaseTiming`, Fielding `Move`/`Sprint`/`Dive`/`Throw` `system_design/input_action_maps.md:16`.
- Gestures `system_design/input_action_maps.md:25` tap/hold/swipe 40px min /120px intent /150ms hold /500ms long press; swipe maps to `GDD.md:165` shot direction 360 deg + power duration `GDD.md:176`.
- Package Input System 1.11.x `TDD.md:5` UI nav via `Navigate`/`Submit` `system_design/input_action_maps.md:16` persistent `UIRoot` `system_design/ui_architecture_and_navigation.md:16`.

### 4.2 Controller Mapping

Controller support should map to the same gameplay concepts:

- footwork,
- shot direction,
- shot type,
- bowling choice,
- fielding control,
- menu navigation.

### 4.3 Input Configuration (persisted via `ISaveService` `system_design/service_interfaces.md:16`)

- Sensitivity 0.5-2.0, `handoff_radius_m` override `system_design/remote_config_and_analytics.md:8` 3.5 fallback, camera lock per mode `ScriptableObjects/Cameras/` `unity_ai_workflow_and_project_structure.md:73`.
- Controller maps same actions `system_design/input_action_maps.md:40` reads actions not device.
- Saved in `profiles/<profileId>/profile.json` `system_design/save_schema_versioning.md:16` + cloud sync field-merge `system_design/save_schema_versioning.md:46`.

## 5. Tutorial Design

### 5.1 Tutorial Order (`player_onboarding_and_tutorial_flow.md:59` + `system_design/state_machines_and_event_model.md:33`)

- Flow `Boot->Home->Tutorial` `system_design/state_machines_and_event_model.md:16` `OnTutorialCompleted` gates `profile.json` tutorialCompleted `system_design/save_schema_versioning.md:33`.
- Steps `player_onboarding_and_tutorial_flow.md:59` defend->single->gap->aggressive->mistime; prompt via `IAnalyticsService.TrackEvent("tutorial_completed")` `system_design/remote_config_and_analytics.md:34`.

### 5.2 Tutorial Tasks

The tutorial should give the player actual actions to complete:

- defend a ball,
- score a run,
- bowl a target ball,
- complete a fielding action,
- change camera,
- enter a match,
- view rewards.

### 5.3 Tutorial UX Rules

- keep prompts short,
- show one lesson at a time,
- use live gameplay instead of text-only explanation,
- make the tutorial skippable for returning players,
- allow the player to jump into Nets or Quick Match quickly.

## 6. Expected Output

- New players can learn quickly.
- Camera choices are meaningful.
- Input feels usable on mobile and controller.
- The tutorial gets the player into real play without confusion.

## 7. Dependencies

- Feature 02 animation pipeline
- Feature 03 scene pipeline
- Feature 04 gameplay
- `game_flow_and_camera_design.md`
- `player_onboarding_and_tutorial_flow.md`
- `mobile_roadmap.md`

## 8. References

- [game_flow_and_camera_design.md](../game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](../player_onboarding_and_tutorial_flow.md)
- [mobile_roadmap.md](../mobile_roadmap.md)
- [animation_requirements.md](../animation_requirements.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- build the camera mode controller,
- build the input abstraction,
- build the tutorial state machine,
- build tutorial objectives,
- build camera settings UI,
- build input settings UI,
- build onboarding validation tests.

## 10. Expected Tests

- tutorial completion tests,
- camera switching tests,
- touch input response tests,
- controller input mapping tests,
- settings persistence tests,
- return-player skip path tests.

## 11. Exit Criteria

- The player can complete the tutorial.
- The camera system is functional.
- The controls are understandable.
- The game can onboard a first-time user.

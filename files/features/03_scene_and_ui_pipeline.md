# Feature 03: Scene and UI Pipeline

## 1. What This Feature Is

This feature creates the app structure and presentation flow.

It is responsible for:

- moving the player through the app,
- showing the right UI at the right time,
- loading the right scene for the right job,
- keeping presentation separate from gameplay logic.

The result should feel like a real mobile game app, not a collection of disconnected screens.

## 2. Why This Feature Matters

If this feature is weak, the game will feel confusing even if the gameplay is good.

The player must always know:

- where they are,
- what they can do next,
- how to start a match,
- how to recover from interruptions,
- how to get back to play quickly.

## 3. Scene Architecture

### 3.1 Boot Scene

Purpose:

- initialize services,
- load config,
- check version,
- route to the next scene.

### 3.2 Login / Profile Scene

Purpose:

- profile selection,
- guest fallback,
- account login,
- first-time setup.

### 3.3 Home Menu Scene

Purpose:

- navigate to all major modes,
- show resume and rewards,
- show profile summary.

### 3.4 Tutorial Scene

Purpose:

- guided learning,
- step-by-step practice,
- onboarding flow.

### 3.5 Nets Scene

Purpose:

- batting practice,
- bowling practice,
- timing and camera tuning.

### 3.6 Match Setup Scene

Purpose:

- team selection,
- camera selection,
- difficulty selection,
- assist selection.

### 3.7 Match Play Scene

Purpose:

- active cricket gameplay,
- HUD,
- scorekeeping,
- state display.

### 3.8 Replay Scene

Purpose:

- highlight playback,
- wicket replay,
- boundary replay.

### 3.9 Results Scene

Purpose:

- summary,
- rewards,
- unlocks,
- next action.

### 3.10 Customize Scene

Purpose:

- visual settings,
- camera settings,
- controls,
- accessibility,
- profile appearance.

### 3.11 Online Lobby Scene

Purpose:

- matchmaking,
- invites,
- team readying,
- connection state.

## 4. UI Architecture (locked: uGUI + Stack per `system_design/ui_architecture_and_navigation.md:8`)

### 4.1 Mobile Readability (tier budgets `system_design.md:188`)

- Tap target >=44dp, text 14sp min, contrast 4.5:1. Low tier single-pass UI, atlas 1024 `system_design.md:188`.

### 4.2 Data-Driven UI

- Panels bind to SO `ScriptableObjects/Modes/...` `unity_ai_workflow_and_project_structure.md:73` + `IContentService` `system_design/service_interfaces.md:42` + `IRemoteConfigService` `system_design/remote_config_and_analytics.md:8`. No hard-coded mode logic `feature_implementation_master.md:290`.

### 4.3 UI Groups + Navigation

Stack model `system_design/ui_architecture_and_navigation.md:16`: `UIRoot` persistent `DontDestroyOnLoad` holds modal stack + toast + loading overlay. Layers `system_design/ui_architecture_and_navigation.md:34` full-screen/modal/HUD/transient. Back nav consistent, `Home` recoverable `system_design/ui_architecture_and_navigation.md:39`. Scene owns HUD `scene_by_scene_setup.md:137` `Match_Play` score overlay.

## 5. Presentation Systems

### 5.1 Cinemachine 3.x (`TDD.md:8` pinned)

Blends 0.3s `system_design/physics_tick_and_reconciliation.md:16` 30Hz sim vs render-rate Animator `TDD.md:93`, modes per `scene_by_scene_setup.md:316` `Match_Play` 5 cameras + `Replay` `game_flow_and_camera_design.md:272` FOV 60/70.

### 5.2 Timeline

Intro/wicket/replay packages `animation_and_scene_pipeline_roadmap.md:279` drive `Results` `scene_by_scene_setup.md:360` presentation camera `features/03/01_boot_login_and_home_flow.md:28`.

### 5.3 Additive Loading

`Boot` (always) + `Home_Menu` additive `scene_by_scene_setup.md:212`, `Match_Play` heavy; heavy `Stadiums` via Addressables `system_design/addressables_grouping.md:16`. Validate `UIRoot` survives `system_design/ui_architecture_and_navigation.md:43`.

### 5.3 Animated Backgrounds

Use them carefully:

- keep them lightweight,
- keep them clean,
- avoid excessive mobile cost.

## 6. Implementation Tasks

### 6.1 Scene Bootstrapping

- Create the scene list.
- Add a persistent bootstrap layer.
- Wire scene transitions.
- Ensure loading states are graceful.

### 6.2 Menu Flow

- Build the home menu.
- Add resume.
- Add mode navigation.
- Add profile and rewards visibility.

### 6.3 Gameplay HUD

- Build score display.
- Build over/innings display.
- Build shot and timing feedback display.
- Build pause and resume controls.

### 6.4 Results and Replay

- Build results summary.
- Build reward reveal.
- Build replay entry and exit flow.

## 7. Expected Output

- The app flow is understandable.
- Scene transitions are stable.
- UI works on mobile.
- Players can reach gameplay quickly.
- Presentation is clean and readable.

## 8. Dependencies

- Feature 00 foundation
- Feature 01 data layer
- Feature 02 animation pipeline
- `scene_by_scene_setup.md`
- `game_flow_and_camera_design.md`
- `player_onboarding_and_tutorial_flow.md`

## 9. References

- [scene_by_scene_setup.md](../scene_by_scene_setup.md)
- [game_flow_and_camera_design.md](../game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](../player_onboarding_and_tutorial_flow.md)
- [unity_ai_workflow_and_project_structure.md](../unity_ai_workflow_and_project_structure.md)

## 10. AI Agent Tasks

An AI agent working on this feature should:

- scaffold scenes,
- build UI prefabs,
- create navigation flow scripts,
- create HUD controllers,
- create menu data bindings,
- create replay/result controllers,
- create loading and transition helpers.

## 11. Expected Tests

- scene load tests,
- menu navigation tests,
- HUD visibility tests,
- results flow tests,
- replay entry/exit tests,
- mobile UI scaling checks.

## 12. Exit Criteria

- All major scenes exist.
- Scene responsibilities are clear.
- UI works on mobile.
- The app flow is understandable.
- The player can navigate the game without confusion.

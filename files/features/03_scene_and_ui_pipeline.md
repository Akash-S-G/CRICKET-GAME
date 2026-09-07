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

## 4. UI Architecture

### 4.1 Mobile Readability

- Build for small screens first.
- Use large enough tap targets.
- Keep text simple and high contrast.
- Avoid burying the player in nested menus.

### 4.2 Data-Driven UI

- UI panels should read from data objects.
- UI should not own match logic.
- Mode cards, rewards, and settings should come from the data layer.

### 4.3 UI Groups

Recommended groups:

- navigation UI,
- match HUD,
- results UI,
- tutorial UI,
- settings UI,
- reward UI,
- lobby UI,
- debug UI.

## 5. Presentation Systems

### 5.1 Cinemachine

Use Cinemachine for:

- follow cameras,
- broadcast framing,
- camera transitions,
- replay shots,
- scene movement.

### 5.2 Timeline

Use Timeline for:

- intro shots,
- wicket sequences,
- replay motion,
- result presentation,
- menu ambience.

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

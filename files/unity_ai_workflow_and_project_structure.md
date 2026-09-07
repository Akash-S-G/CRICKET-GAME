# Unity AI Workflow and Project Structure

This document turns the Unity plan into a practical production workflow. It explains the folder structure, scene hierarchy, code architecture, and how to use AI agents safely and effectively while building the game.

## 1. Goal

The goal is to make the project:

- easy to navigate,
- easy to automate with AI tools,
- safe to expand into online multiplayer later,
- mobile-first from the start,
- maintainable by humans and agents together.

The rule is simple: use AI to speed up development, but keep the architecture structured enough that AI changes stay reviewable.

## 2. Recommended Unity Project Structure

Use a clean separation between code, data, scenes, prefabs, and art.

```text
Assets/
  _Project/
    Art/
      Characters/
      Environments/
      UI/
      VFX/
      Icons/
    Audio/
      Music/
      SFX/
      Commentary/
    Animations/
      Clips/
      Controllers/
      OverrideControllers/
      Rigs/
      Timelines/
    Prefabs/
      Players/
      Ball/
      Bat/
      Fielders/
      Cameras/
      UI/
      Environment/
    Scenes/
      Boot/
      Login/
      Home/
      Tutorial/
      Nets/
      MatchSetup/
      MatchPlay/
      Replay/
      Results/
      Customize/
      OnlineLobby/
    Scripts/
      Core/
      Domain/
      Gameplay/
      UI/
      Networking/
      Services/
      Animation/
      Cameras/
      Input/
      Save/
      Analytics/
      Tools/
    ScriptableObjects/
      Modes/
      Players/
      Pitches/
      Grounds/
      Cameras/
      Progression/
      Economy/
      Tutorials/
    Addressables/
      Catalog/
      ContentGroups/
      Remote/
    Tests/
      EditMode/
      PlayMode/
    Resources/
    Shaders/
    Materials/
    Gizmos/
```

## 3. Folder Rules

### 3.1 Art

Use `Art` only for source assets or imported final art. Do not put gameplay scripts here.

### 3.2 Animations

Use `Animations` for:

- clips,
- animator controllers,
- override controllers,
- rigging data,
- timeline sequences.

### 3.3 Prefabs

Use `Prefabs` for reusable gameplay objects.

Examples:

- batter prefab,
- bowler prefab,
- fielder prefab,
- ball prefab,
- wicket prefab,
- camera rig prefab,
- UI panel prefab.

### 3.4 Scenes

Keep each scene small and specific.

### 3.5 Scripts

Split scripts by responsibility, not by team preference.

### 3.6 ScriptableObjects

Store all authored tuning data here so it can be edited without changing code.

### 3.7 Tests

Keep edit-mode and play-mode tests separate.

## 4. Core Scene Hierarchy

This is the recommended top-level hierarchy for the main gameplay scene.

```text
MatchPlay
  ├── SceneRoot
  ├── Systems
  │   ├── MatchManager
  │   ├── RulesEngine
  │   ├── BallSimulation
  │   ├── FieldingManager
  │   ├── CameraManager
  │   ├── InputManager
  │   ├── AudioManager
  │   ├── UIManager
  │   ├── SaveManager
  │   └── AnalyticsManager
  ├── Players
  │   ├── BattingTeam
  │   ├── FieldingTeam
  │   └── NPCBackfill
  ├── Ball
  ├── Pitch
  ├── Ground
  ├── Cameras
  ├── UI
  ├── VFX
  ├── Audio
  └── Debug
```

## 5. Scene-by-Scene Setup

### 5.1 Boot Scene

Purpose:

- initialize services,
- load remote config,
- initialize save data,
- validate versioning,
- route to the correct next scene.

Hierarchy:

```text
Boot
  ├── BootRoot
  ├── ServiceBootstrap
  ├── LoadingUI
  ├── VersionCheck
  └── ErrorFallbackUI
```

### 5.2 Login / Profile Scene

Purpose:

- profile selection,
- guest fallback,
- account login,
- first-time user creation.

Hierarchy:

```text
Login_Profile
  ├── ProfileRoot
  ├── ProfileList
  ├── CreateProfilePanel
  ├── LoginPanel
  └── OfflineFallbackPanel
```

### 5.3 Home Menu Scene

Purpose:

- navigation hub,
- daily rewards,
- quick resume,
- mode selection.

Hierarchy:

```text
Home_Menu
  ├── MenuRoot
  ├── AnimatedBackground
  ├── ProfileHeader
  ├── ModeCards
  ├── DailyRewardPanel
  ├── ResumePanel
  ├── SettingsShortcut
  └── AmbientAudio
```

### 5.4 Tutorial Scene

Purpose:

- teach batting,
- teach bowling,
- teach fielding,
- teach camera use,
- teach progression basics.

Hierarchy:

```text
Tutorial
  ├── TutorialRoot
  ├── LessonController
  ├── ObjectiveTracker
  ├── PracticeArena
  ├── PromptOverlay
  ├── SuccessFailUI
  └── CameraGuideUI
```

### 5.5 Nets Scene

Purpose:

- first playable batting practice,
- bowling machine or AI bowler,
- camera testing,
- timing tuning.

Hierarchy:

```text
Nets
  ├── NetsRoot
  ├── PracticePitch
  ├── BowlingMachine
  ├── BatterSpawn
  ├── PracticeHUD
  ├── ResetControls
  ├── CameraSet
  └── DebugReadout
```

### 5.6 Match Setup Scene

Purpose:

- choose squad,
- pick roles,
- select camera,
- select assist and difficulty settings.

Hierarchy:

```text
Match_Setup
  ├── SetupRoot
  ├── SquadSelector
  ├── RoleSelector
  ├── CameraSelector
  ├── DifficultySelector
  ├── AssistSettings
  ├── MatchSummary
  └── StartMatchButton
```

### 5.7 Match Play Scene

Purpose:

- full cricket simulation,
- ball-by-ball play,
- rules resolution,
- fielding handoff,
- score updates.

Hierarchy:

```text
Match_Play
  ├── MatchRoot
  ├── Stadium
  ├── Pitch
  ├── Ground
  ├── PlayerActors
  ├── Ball
  ├── MatchSystems
  ├── Cameras
  ├── HUD
  ├── VFX
  ├── Audio
  └── Debug
```

### 5.8 Replay Scene

Purpose:

- highlights,
- wickets,
- boundaries,
- close calls,
- presentation replays.

Hierarchy:

```text
Replay
  ├── ReplayRoot
  ├── ReplayTimeline
  ├── ReplayCameraRig
  ├── PlaybackControls
  ├── ReplayStats
  └── ExitReplayButton
```

### 5.9 Results Scene

Purpose:

- score summary,
- progression rewards,
- unlocks,
- next action.

Hierarchy:

```text
Results
  ├── ResultsRoot
  ├── ScoreSummary
  ├── PlayerPerformance
  ├── RewardsPanel
  ├── UnlockPanel
  └── NextActions
```

### 5.10 Customize Scene

Purpose:

- cosmetics,
- camera preferences,
- control settings,
- accessibility.

Hierarchy:

```text
Customize
  ├── CustomizeRoot
  ├── CosmeticsPanel
  ├── CameraSettingsPanel
  ├── ControlSettingsPanel
  ├── AccessibilityPanel
  └── SaveChangesButton
```

### 5.11 Online Lobby Scene

Purpose:

- matchmaking,
- invite flow,
- lobby readiness,
- network status.

Hierarchy:

```text
OnlineLobby
  ├── LobbyRoot
  ├── MatchmakingPanel
  ├── LobbyList
  ├── InvitePanel
  ├── ReadyStatePanel
  └── NetworkStatusUI
```

## 6. Code Architecture

Use a layered architecture. Do not mix domain logic with UI and scene behavior.

### 6.1 Domain Layer

Contains:

- match state machine,
- rules engine,
- ball physics model,
- shot resolution,
- wicket logic,
- progression rules.

Suggested namespaces:

- `Game.Domain`
- `Game.Domain.Rules`
- `Game.Domain.Match`
- `Game.Domain.Physics`
- `Game.Domain.Progression`

### 6.2 Gameplay Layer

Contains:

- batters,
- bowlers,
- fielders,
- keeper logic,
- AI controllers,
- handoff logic,
- input translation.

Suggested namespaces:

- `Game.Gameplay`
- `Game.Gameplay.Batting`
- `Game.Gameplay.Bowling`
- `Game.Gameplay.Fielding`
- `Game.Gameplay.AI`
- `Game.Gameplay.Controllers`

### 6.3 Presentation Layer

Contains:

- HUD,
- menus,
- camera control,
- animation glue,
- VFX,
- audio,
- replays.

Suggested namespaces:

- `Game.Presentation`
- `Game.Presentation.UI`
- `Game.Presentation.Camera`
- `Game.Presentation.Animation`
- `Game.Presentation.Replay`

### 6.4 Services Layer

Contains:

- authentication,
- save/load,
- analytics,
- remote config,
- matchmaking,
- cloud sync,
- addressable loading.

Suggested namespaces:

- `Game.Services`
- `Game.Services.Save`
- `Game.Services.Auth`
- `Game.Services.Analytics`
- `Game.Services.Config`
- `Game.Services.Network`

### 6.5 Tools Layer

Contains:

- debug menus,
- validation tools,
- editor helpers,
- test utilities,
- scene bootstrap helpers.

Suggested namespaces:

- `Game.Tools`
- `Game.Tools.Debug`
- `Game.Tools.Editor`
- `Game.Tools.Validation`

## 7. Recommended Classes

### Core Systems

- `GameBootstrapper`
- `ServiceRegistry`
- `SessionController`
- `MatchManager`
- `RulesEngine`
- `BallSimulationController`
- `FieldingManager`
- `CameraDirector`
- `InputRouter`
- `SaveService`
- `AnalyticsService`

### Player and AI

- `PlayerActor`
- `PlayerControllerBase`
- `HumanController`
- `AIController`
- `BattingController`
- `BowlingController`
- `FieldingController`
- `KeeperController`

### Presentation

- `HudController`
- `MenuController`
- `ResultsController`
- `ReplayController`
- `TutorialController`
- `CameraModeController`

### Data

- `ModeConfig`
- `PlayerProfileData`
- `GroundData`
- `PitchData`
- `CameraConfig`
- `TutorialStepData`
- `ProgressionRewardData`

## 8. Best Practices for AI Agents

### 8.1 One Agent, One Slice

Do not ask multiple agents to edit the same system at once.

Good slices:

- one scene,
- one controller,
- one data schema,
- one UI flow,
- one animation state system.

### 8.2 Start From the Doc

Every agent task should begin with the relevant doc.

For example:

- scene work starts from `scene_by_scene_setup.md`
- camera work starts from `game_flow_and_camera_design.md`
- animation work starts from `animation_clip_inventory.md`
- technical work starts from `system_design.md`

### 8.3 Ask for Small Output

Use prompts that produce a small, reviewable result.

Good:

- “Create the Boot scene bootstrap and service initialization flow.”
- “Create the camera mode controller for Nets.”
- “Create the match setup panel with data-driven options.”

Bad:

- “Build the whole game.”

### 8.4 Keep AI Output Deterministic

Use AI to generate:

- boilerplate,
- glue code,
- editor helpers,
- validation scripts,
- simple state wiring.

Do not let AI invent:

- core match rules,
- network authority behavior,
- animation timing behavior,
- balance changes,
- final camera defaults,
- production save logic without review.

### 8.5 Review Every Agent Change

Check:

- does it match the docs,
- does it compile,
- does it keep mobile fast,
- does it preserve offline behavior,
- does it remain testable,
- does it create hidden coupling.

## 9. Unity MCP Workflow

### Use Unity MCP for

- scene inspection,
- GameObject changes,
- component updates,
- script generation,
- debugging the active scene,
- checking console output,
- editing scene-based content.

### Use Terminal/CLI for

- git operations,
- test runs,
- build scripts,
- schema validation,
- package management,
- asset pipeline checks.

### Use AI Editors for

- code generation,
- refactors,
- documentation support,
- prompt-driven scene setup,
- script iteration.

## 10. Production Workflow

### Step 1: Define the slice

Write the feature in the design doc first.

### Step 2: Create the asset path

Decide:

- scene,
- prefab,
- script,
- data asset,
- animation clips,
- camera mode.

### Step 3: Generate the scaffold

Use AI to create the initial scripts and Unity objects.

### Step 4: Connect data

Use ScriptableObjects and JSON schemas to drive behavior.

### Step 5: Test in Unity

Verify:

- play mode,
- scene load,
- input,
- camera,
- animation,
- save/load,
- transition flow.

### Step 6: Harden

Profile, fix memory, reduce allocations, and clean transitions.

## 11. Prompt Templates for AI Agents

### 11.1 Scene Scaffold Prompt

```text
Create the Unity scene scaffold for [scene name].
Use the project docs as the source of truth.
Include the required root objects, persistent services, UI panels, and camera setup.
Keep the code modular and mobile-first.
```

### 11.2 Gameplay System Prompt

```text
Implement [system name] as a separate gameplay module.
It must follow the domain/presentation/service split in the docs.
Do not mix UI logic with rules logic.
Keep the system ready for future multiplayer.
```

### 11.3 Animation Prompt

```text
Create the animation state setup for [animation area].
Use the clip inventory and rigging docs.
Keep clips cricket-specific and support timing variants.
Return the Animator parameters, transitions, and any required rigging hooks.
```

### 11.4 Camera Prompt

```text
Implement the camera mode system for [scene or mode].
Support the documented camera types and defaults.
Use Cinemachine and keep switching fast for mobile.
```

### 11.5 UI Prompt

```text
Create the UI flow for [screen].
Keep the screen readable on mobile, data-driven, and easy to navigate.
Use the player onboarding and game flow docs.
```

### 11.6 Validation Prompt

```text
Create validation scripts or tests for [feature].
Check the main edge cases, performance risks, and state transitions.
The result should be useful in CI and in the Unity Editor.
```

## 12. Checklist Before Each Implementation

- [ ] Relevant docs read
- [ ] Scene target identified
- [ ] Data asset shape decided
- [ ] AI tool selected
- [ ] Test plan defined
- [ ] Multiplayer implications checked
- [ ] Mobile performance risk checked
- [ ] Animation implications checked
- [ ] Transition flow checked

## 13. Checklist Before Merge

- [ ] Compiles cleanly
- [ ] Scene loads correctly
- [ ] No broken references
- [ ] No hidden hard-coded mode logic
- [ ] Mobile path preserved
- [ ] Offline fallback preserved
- [ ] Multiplayer path still feasible later
- [ ] Docs updated if behavior changed
- [ ] Tests or validation scripts added

## 14. Final Rule

If the change cannot be expressed clearly in:

- one scene,
- one data asset,
- one controller,
- one test,
- and one doc update,

then the change is probably too large and should be split before an AI agent works on it.

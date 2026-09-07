# Master Implementation Plan

This is the execution plan for building the game from zero to a production-ready mobile cricket product that can later support online multiplayer.

It is written for humans and AI agents working together. The docs in this repo are the source of truth.

## 1. Purpose

The implementation plan has four goals:

- make the project start correctly,
- make the build process stable,
- make the game playable early,
- keep the architecture safe for future multiplayer and live updates.

The game should be built in slices. Each slice must be playable, testable, and documented before the next slice starts.

## 2. Source of Truth Documents

Before implementing anything, keep these docs open and aligned:

- [GDD.md](GDD.md)
- [system_design.md](system_design.md)
- [TECH_STACK.md](TECH_STACK.md)
- [development_plan.md](development_plan.md)
- [animation_and_scene_pipeline_roadmap.md](animation_and_scene_pipeline_roadmap.md)
- [animation_clip_inventory.md](animation_clip_inventory.md)
- [scene_by_scene_setup.md](scene_by_scene_setup.md)
- [game_flow_and_camera_design.md](game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](player_onboarding_and_tutorial_flow.md)
- [unity_ai_workflow_and_project_structure.md](unity_ai_workflow_and_project_structure.md)
- [TDD.md](TDD.md)
- [rules_engine_spec.md](rules_engine_spec.md)
- [controller_handoff_spec.md](controller_handoff_spec.md)
- [mobile_roadmap.md](mobile_roadmap.md)
- [roadmap.md](roadmap.md)
- [risks.md](risks.md)
- [risk_resolution.md](risk_resolution.md)

## 3. Implementation Philosophy

1. Build the domain first, presentation second.
2. Keep all mode rules data-driven.
3. Use small, reviewable AI-generated slices.
4. Make every phase playable before moving on.
5. Keep mobile performance and short sessions as first-class constraints.
6. Make offline play solid before online multiplayer.
7. Make the animation pipeline cricket-specific, not generic.
8. Keep every feature compatible with future authoritative multiplayer.

## 4. Required Project Foundation

### 4.1 Repo and Unity Setup

Before gameplay work starts:

- initialize Unity 6 LTS project,
- set URP,
- create the folder structure from [unity_ai_workflow_and_project_structure.md](unity_ai_workflow_and_project_structure.md),
- add the base scenes,
- set up Git LFS for binary assets,
- configure `.gitignore`,
- add the docs index and README,
- connect Unity MCP,
- verify the AI toolchain,
- establish test automation entry points.

### 4.2 Build and Environment Setup

Set up:

- development build target,
- mobile build target,
- local test profile,
- debug logging profile,
- scene bootstrap flow,
- service bootstrap flow,
- asset loading flow,
- save/load location,
- remote config hooks,
- analytics hooks.

### 4.3 Required Technical Decisions

These choices must be locked early:

- Unity 6 LTS
- URP
- data-driven mode configs
- server-authoritative logic for future multiplayer
- Addressables for optional/large content
- Cinemachine for cameras
- Timeline for cinematic sequences
- Animation Rigging for cricket motion correction
- local save + cloud sync path

## 5. Phase 0: Documentation and Architecture Lock

### Objective

Make sure the game is fully spec’d before code grows.

### Tasks

- confirm all design docs are present,
- confirm data schemas exist,
- define class boundaries,
- define scene responsibilities,
- define animation clip inventory,
- define camera modes,
- define multiplayer strategy,
- define progression strategy,
- define the AI tool workflow,
- define test strategy.

### Exit Criteria

- a new engineer or AI agent can understand the entire project from the docs alone,
- the docs do not conflict,
- the initial scene list and system boundaries are stable.

## 6. Phase 1: Project Initialization

### Objective

Make the Unity project open, run, and build cleanly.

### Tasks

- create the Unity project,
- apply folder structure,
- import baseline packages,
- configure input system,
- configure URP,
- create bootstrap scene,
- create placeholder menu scene,
- create placeholder match scene,
- wire persistent services,
- verify play mode startup,
- verify mobile build target,
- verify editor automation via MCP.

### AI Usage

Use AI for:

- initial folder scaffolding,
- bootstrap service code,
- editor utilities,
- initial scene setup scripts,
- basic UI shell code.

### Tests

- project opens without errors,
- build target works,
- scene transitions work,
- persistent services survive scene loads,
- log output is clean.

### Exit Criteria

- the app can boot to the menu,
- scenes can load without manual editor repair,
- the repo is ready for feature work.

## 7. Phase 2: Data and Service Layer

### Objective

Build the foundation that all gameplay will use.

### Tasks

- implement mode config loading,
- implement player profile data,
- implement pitch and ground data,
- implement camera config data,
- implement progression data,
- implement local save service,
- implement remote config service,
- implement analytics hooks,
- implement audio manager,
- implement session controller.

### Best Practices

- keep config data in ScriptableObjects and JSON schemas,
- avoid hard-coded values in gameplay scripts,
- use events for state changes,
- keep save/load logic separate from gameplay logic,
- keep service interfaces stable.

### Tests

- config loads from data assets,
- save and load round-trip,
- profile data persists,
- remote config values apply correctly,
- analytics events fire on expected transitions.

### Exit Criteria

- all core data types can be loaded and saved,
- the game has a stable service layer,
- later gameplay can consume these services cleanly.

## 8. Phase 3: Animation Pipeline

### Objective

Build the cricket motion foundation before deep gameplay polish.

### Tasks

- create humanoid player rig standard,
- import or create base locomotion clips,
- create batting stance clips,
- create bowling run-up and delivery clips,
- create fielding and wicketkeeper clips,
- build Animator Controllers,
- build blend trees,
- set up Animation Rigging,
- create Timeline sequences for intro/replay/result moments,
- integrate clip inventory from [animation_clip_inventory.md](animation_clip_inventory.md).

### Required Work Order

1. Base locomotion
2. Batting stance and shots
3. Bowling actions
4. Fielding pickup, throw, catch
5. Wicketkeeper clips
6. Celebration and reaction clips
7. Cinematic and replay motion

### AI Usage

Use AI for:

- animator parameter scaffolds,
- state machine boilerplate,
- rigging setup scripts,
- clip naming helpers,
- validation tools,
- Timeline helper code.

### Tests

- clips retarget correctly,
- batting timing variants are distinguishable,
- bowling forms are readable,
- fielding motions do not break foot placement,
- camera transitions do not fight the animation,
- replay clips look acceptable in real-time and slow motion.

### Exit Criteria

- batting, bowling, and fielding all have usable animation states,
- the game no longer feels like placeholder movement,
- the motion system is ready for gameplay tuning.

## 9. Phase 4: Scene Pipeline

### Objective

Build the scene structure that the full game will run on.

### Tasks

- create `Boot`,
- create `Login_Profile`,
- create `Home_Menu`,
- create `Tutorial`,
- create `Nets`,
- create `Match_Setup`,
- create `Match_Play`,
- create `Replay`,
- create `Results`,
- create `Customize`,
- create `OnlineLobby`,
- add persistent service loading,
- add additive scene loading where needed,
- connect scene transitions to the session controller.

### Best Practices

- keep each scene single-purpose,
- keep persistent services in bootstrap,
- make loading asynchronous,
- keep mobile boot time short,
- keep scene transitions safe and recoverable.

### Tests

- each scene loads independently,
- transition order works end-to-end,
- persistent systems survive scene swaps,
- replay and results do not corrupt gameplay state,
- mobile back navigation works.

### Exit Criteria

- the full app flow can move from boot to menu to tutorial to practice to match to results,
- scene responsibilities are clean and stable.

## 10. Phase 5: Core Gameplay Loop

### Objective

Make cricket playable from end to end.

### Tasks

- implement ball simulation,
- implement batting timing windows,
- implement shot selection,
- implement bowling release logic,
- implement fielding control handoff,
- implement wicket logic,
- implement extras and innings rules,
- implement over changes and innings changes,
- implement score updates,
- implement match result resolution.

### Best Practices

- keep rules logic pure where possible,
- keep ball resolution deterministic,
- make UI react to game events,
- separate physics from presentation,
- use the rules engine as the source of truth.

### Tests

- shot timing outcomes are correct,
- wickets resolve correctly,
- extras resolve correctly,
- innings completion works,
- over progression works,
- fielding handoff works,
- score updates are correct.

### Exit Criteria

- a solo human-vs-AI match can start, play, and finish,
- Nets mode feels like real cricket practice,
- rules and state transitions are stable.

## 11. Phase 6: Mobile MVP

### Objective

Make the game genuinely good on a phone.

### Tasks

- implement touch-first controls,
- implement the tutorial flow,
- implement Quick Match,
- implement save/resume,
- implement offline play,
- implement mobile camera modes,
- implement performance tiers,
- implement clean HUD and menus,
- implement short-session flow,
- implement onboarding and return-player flow.

### Best Practices

- prioritize clarity over visual density,
- keep controls simple on first launch,
- allow one-handed or low-friction play where possible,
- ensure low-end devices still run the game,
- reduce battery-heavy rendering,
- avoid large install size early.

### Tests

- tutorial completion,
- quick match startup,
- offline session continuity,
- resume behavior,
- UI readability on small screens,
- device tier performance,
- input responsiveness.

### Exit Criteria

- the game is good enough to ship as a mobile product,
- the player can learn and play quickly,
- the experience holds up offline.

## 12. Phase 7: Multiplayer Readiness

### Objective

Prepare the game for future online multiplayer.

### Tasks

- add local/LAN multiplayer proof,
- validate control handoff under latency,
- validate server authority assumptions,
- validate AI backfill,
- validate session ownership and disconnect behavior,
- validate match state replication,
- prepare dedicated server or multiplayer service integration.

### Best Practices

- keep gameplay authoritative,
- keep the ball authoritative,
- never let client prediction decide rules,
- make ownership explicit,
- keep offline and online behavior aligned.

### Tests

- latency injection,
- disconnect/reconnect,
- handoff continuity,
- run-out/stumping authority,
- sync correctness,
- match completion under network stress.

### Exit Criteria

- small online matches are reliable,
- the architecture is ready to scale to Main Mode,
- the multiplayer path does not require a rewrite.

## 13. Phase 8: Main Mode and Scale-Up

### Objective

Scale the game to the flagship mode.

### Tasks

- scale to full player counts,
- add full field management,
- optimize replication,
- revisit NGO versus Photon Fusion only if necessary,
- tune performance and bandwidth,
- validate match length and rules accuracy at scale.

### Tests

- full innings simulation,
- full online match completion,
- bandwidth profiling,
- server load profiling,
- correctness under network stress.

### Exit Criteria

- 11v11 works reliably,
- the game remains playable and accurate,
- the architecture still feels maintainable.

## 14. Phase 9: Progression and Live Features

### Objective

Add retention and long-term play features.

### Tasks

- add rewards,
- add cosmetics,
- add missions,
- add challenge ladders,
- add replay/highlight flow,
- add seasonal content hooks,
- add cloud sync refinements,
- add analytics for retention tuning.

### Best Practices

- do not make competitive modes pay-to-win,
- keep progression useful in short sessions,
- keep live content data-driven,
- keep event systems easy to update.

### Exit Criteria

- players have reasons to return,
- progression does not damage fairness,
- live updates can be managed without code churn.

## 15. Testing Strategy

Testing should happen at every layer.

### 15.1 Edit-Mode Tests

- data parsing,
- state machine transitions,
- config validation,
- save/load logic,
- rules logic,
- camera config logic.

### 15.2 Play-Mode Tests

- scene transitions,
- tutorial flow,
- batting timing,
- bowling release,
- fielding handoff,
- camera switching,
- replay flow,
- result flow.

### 15.3 Mobile Tests

- frame rate,
- battery cost,
- memory use,
- input latency,
- low-end device support,
- app resume behavior,
- UI scale behavior.

### 15.4 Multiplayer Tests

- latency injection,
- disconnect/reconnect,
- host/server failures,
- ownership transfer,
- ball state synchronization,
- run-out timing correctness.

### 15.5 Production Validation

- build smoke tests,
- analytics event validation,
- remote config validation,
- content load validation,
- crash reporting checks.

## 16. AI Agent Operating Model

### 16.1 Agent Rules

- one feature slice at a time,
- one primary agent per slice,
- docs first, code second,
- keep outputs small and reviewable,
- never skip validation,
- update docs when behavior changes.

### 16.2 Good Agent Slices

- one scene scaffold,
- one service class,
- one controller,
- one UI flow,
- one animation system,
- one test bundle,
- one data schema.

### 16.3 Bad Agent Slices

- “build the whole game,”
- giant multi-system refactors without tests,
- gameplay and UI in one large change,
- changing the architecture while also adding content.

### 16.4 Required Agent Context

Each agent task should include:

- relevant design docs,
- target phase,
- target scene or system,
- expected output shape,
- test requirements,
- mobile constraints,
- multiplayer constraints if relevant.

## 17. Required Documentation Set

These docs are required to keep the project context complete:

- [README.md](../README.md)
- [CONTRIBUTING.md](../CONTRIBUTING.md)
- [files/INDEX.md](INDEX.md)
- [GDD.md](GDD.md)
- [system_design.md](system_design.md)
- [TECH_STACK.md](TECH_STACK.md)
- [development_plan.md](development_plan.md)
- [roadmap.md](roadmap.md)
- [mobile_roadmap.md](mobile_roadmap.md)
- [game_flow_and_camera_design.md](game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](player_onboarding_and_tutorial_flow.md)
- [animation_and_scene_pipeline_roadmap.md](animation_and_scene_pipeline_roadmap.md)
- [animation_clip_inventory.md](animation_clip_inventory.md)
- [scene_by_scene_setup.md](scene_by_scene_setup.md)
- [unity_ai_workflow_and_project_structure.md](unity_ai_workflow_and_project_structure.md)
- [TDD.md](TDD.md)
- [rules_engine_spec.md](rules_engine_spec.md)
- [controller_handoff_spec.md](controller_handoff_spec.md)
- [risks.md](risks.md)
- [risk_resolution.md](risk_resolution.md)
- [competitive_analysis.md](competitive_analysis.md)

## 18. Final Implementation Rule

If a feature is not:

- documented,
- testable,
- data-driven,
- mobile-safe,
- and compatible with future multiplayer,

then it is not ready to be implemented.

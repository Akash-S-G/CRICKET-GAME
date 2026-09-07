# Feature Implementation Master

This file is the execution map for the whole project. It tells an AI agent or human developer what to build, in what order, what each feature depends on, and what the expected output is.

The project should be built in feature slices, not as one large pass.

## 1. How To Use This File

- Start at Feature 0 and do not skip ahead unless the dependencies are already complete.
- Each feature has an overview doc under `files/features/`.
- Each feature also has smaller subfeature docs under `files/features/<feature>/`.
- Read the overview doc first, then read the exact subfeature doc before implementing that slice.
- Use one subfeature doc as the scope boundary for one branch or one AI task.
- Update the relevant subfeature doc if implementation decisions change the intended behavior.

## 2. Git Versioning Strategy

Yes, Git versioning should be used for this project.

### Recommended Git Model

- `main` stays stable and merge-ready.
- Create one branch per feature slice.
- Merge only when the feature’s exit criteria are met.
- Tag milestones with readable release tags.
- Keep commits small and descriptive.

### Branch Naming

- `feat/00-foundation`
- `feat/01-data-services`
- `feat/02-animation-pipeline`
- `feat/03-scenes-ui`
- `feat/04-core-gameplay`
- `feat/05-camera-input-tutorial`
- `feat/06-mobile-mvp`
- `feat/07-multiplayer-readiness`
- `feat/08-testing-ci`

### Version Tagging

Use tags for meaningful checkpoints:

- `v0.1-docs`
- `v0.2-project-setup`
- `v0.3-nets-playable`
- `v0.4-mobile-mvp`
- `v0.5-multiplayer-ready`

### What Git Versioning Is For Here

- AI-safe reviewable changes
- rollback to known-good states
- milestone traceability
- separation of feature slices
- easier debugging across phases

## 3. Feature Order

The order below is intentional. Later features depend on earlier ones.

1. Foundation and Tooling
2. Data and Service Layer
3. Animation Pipeline
4. Scene and UI Pipeline
5. Core Cricket Gameplay
6. Camera, Input, and Tutorial Flow
7. Mobile MVP and Progression
8. Multiplayer Readiness
9. Testing, CI, and Release Hardening

## 4. Feature Breakdown

The overview docs stay at the feature level. The implementation work is split into smaller docs below.

### Feature 0: Foundation and Tooling

Build the Unity project base, repo conventions, AI tooling, and service bootstrap.

Concrete lock docs:

- `features/00/01_project_manifest_and_tooling_lock.md`

### Feature 1: Data and Service Layer

Build the game data backbone:

- mode configs
- player profiles
- pitch and ground data
- camera configs
- progression data
- save/load
- remote config
- analytics
- session services

This feature should define the authoritative data shapes used by every other feature.
Subdocs:

- `features/01/00_profile_field_list_and_versions.md`
- `features/01/01_data_contracts_and_schemas.md`
- `features/01/02_profile_save_session_services.md`
- `features/01/03_remote_config_and_analytics.md`

### Feature 2: Animation Pipeline

Build the cricket motion pipeline:

- locomotion
- batting stance and shots
- bowling run-up and delivery motion
- fielding pickups, throws, catches, dives
- wicketkeeper motion
- umpire signals
- celebrations and reactions
- rigging and Timeline support

This feature should make the game visually believable and gameplay-readable.
Subdocs:

- `features/02/00_rig_source_and_naming.md`
- `features/02/01_rig_retargeting_and_locomotion.md`
- `features/02/02_batting_animation_set.md`
- `features/02/03_bowling_animation_set.md`
- `features/02/04_fielding_keeper_and_presentation.md`
- `features/02/05_animator_timeline_and_events.md`

### Feature 3: Scene and UI Pipeline

Build the scene and UI layer:

- boot flow
- login/profile
- home menu
- tutorial
- Nets
- match setup
- match play
- replay
- results
- customize
- online lobby

This feature should make the app flow understandable and mobile-friendly.
Subdocs:

- `features/03/00_ui_design_tokens_and_hud_layout.md`
- `features/03/01_boot_login_and_home_flow.md`
- `features/03/02_tutorial_nets_and_match_setup.md`
- `features/03/03_match_hud_replay_and_results.md`
- `features/03/04_customize_and_online_lobby.md`
- `features/03/05_audio_haptics_and_feedback.md`

### Feature 4: Core Cricket Gameplay

Build the cricket simulation and match logic:

- ball physics
- batting timing
- shot selection
- bowling execution
- fielding handoff
- wickets
- extras
- overs
- innings
- scoring
- match state machine

This feature should make the game actually play like cricket.
Subdocs:

- `features/04/00_ball_state_and_timing_table.md`
- `features/04/01_ball_physics_and_contact_model.md`
- `features/04/02_batting_outcome_and_shot_model.md`
- `features/04/03_bowling_and_fielding_flow.md`
- `features/04/04_rules_engine_and_match_state.md`

### Feature 5: Camera, Input, and Tutorial Flow

Build the usability layer:

- camera modes
- touch mapping
- controller mapping
- onboarding
- tutorial
- first-run learning
- return-player guidance

This feature should make the game learnable and readable.
Subdocs:

- `features/05/00_action_maps_and_gesture_thresholds.md`
- `features/05/01_camera_modes_and_presentation.md`
- `features/05/02_touch_and_controller_input.md`
- `features/05/03_tutorial_and_first_run_flow.md`

### Feature 6: Mobile MVP and Progression

Build the mobile-first playable version:

- offline play
- quick sessions
- save/resume
- mobile UI polish
- performance tiers
- basic progression
- rewards
- short-session retention

This feature should make the game feel complete on a phone.
Subdocs:

- `features/06/00_mobile_mvp_contract.md`
- `features/06/01_mobile_session_loop.md`
- `features/06/02_progression_rewards_and_unlocks.md`
- `features/06/03_mobile_performance_and_settings.md`
- `features/06/04_progression_numbers_and_unlock_table.md`
- `features/06/05_device_tier_budget.md`

### Feature 7: Multiplayer Readiness

Build the future online path:

- local/LAN proof
- server-authoritative planning
- AI backfill
- control handoff
- disconnect/reconnect behavior
- ownership model
- latency tolerance
- match session replication

This feature should make the game ready to scale into online cricket.
Subdocs:

- `features/07/00_networking_go_no_go_and_budget.md`
- `features/07/01_authority_and_replication_model.md`
- `features/07/02_lobby_and_session_flow.md`
- `features/07/03_disconnect_reconnect_and_backfill.md`
- `features/07/04_authority_matrix_and_replication_table.md`

### Feature 8: Testing, CI, and Release Hardening

Build the quality and release layer:

- edit-mode tests
- play-mode tests
- mobile validation
- multiplayer validation
- build checks
- CI
- telemetry
- release fallback behavior

This feature should make the project safe to ship and maintain.
Subdocs:

- `features/08/00_ci_toolchain_and_commands.md`
- `features/08/01_edit_and_play_mode_tests.md`
- `features/08/02_mobile_validation_and_performance.md`
- `features/08/03_ci_and_release_hardening.md`
- `features/08/04_release_gates_and_observability.md`

## 5. Deliverable Rule

Every feature must ship with:

- what the feature is,
- how to implement it,
- expected output,
- dependencies,
- tests,
- references,
- exit criteria.

## 6. Branch Workflow

For each feature:

1. Read the feature doc.
2. Read the exact subfeature doc for the task.
3. Create a feature branch or task branch for that slice.
4. Implement only that slice.
5. Run tests and Unity validation.
6. Update docs if the design changed.
7. Merge when the exit criteria are met.

## 7. Source Docs

These docs define the project and should stay aligned:

- [GDD.md](GDD.md)
- [system_design.md](system_design.md)
- [TECH_STACK.md](TECH_STACK.md)
- [development_plan.md](development_plan.md)
- [animation_and_scene_pipeline_roadmap.md](animation_and_scene_pipeline_roadmap.md)
- [scene_by_scene_setup.md](scene_by_scene_setup.md)
- [game_flow_and_camera_design.md](game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](player_onboarding_and_tutorial_flow.md)
- [unity_ai_workflow_and_project_structure.md](unity_ai_workflow_and_project_structure.md)
- [milestone_checklist.md](milestone_checklist.md)

## 8. Final Rule

Do not jump to later feature files before the dependencies are ready.
Do not let AI agents work without a feature doc.
Do not merge code that violates the documented architecture.

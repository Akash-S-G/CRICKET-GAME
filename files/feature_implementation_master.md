# Feature Implementation Master

This file is the execution map for the whole project. It tells an AI agent or human developer what to build, in what order, what each feature depends on, and what the expected output is.

The project should be built in feature slices, not as one large pass.

## 1. How To Use This File

- Start at Feature 0 and do not skip ahead unless the dependencies are already complete.
- Each feature has its own doc under `files/features/`.
- Read the feature doc before implementing anything in that area.
- Use the feature doc as the scope boundary for one branch or one AI task.
- Update the feature doc if implementation decisions change the intended behavior.

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

## 4. Feature Overview

### Feature 0: Foundation and Tooling

Build the Unity project base, repo conventions, AI tooling, and service bootstrap.

### Feature 1: Data and Service Layer

Build the mode configs, player profiles, save/load, remote config, analytics, and session services.

### Feature 2: Animation Pipeline

Build the cricket motion pipeline for batting, bowling, fielding, keeper, and presentation clips.

### Feature 3: Scene and UI Pipeline

Build the scene structure, menu flow, HUD flow, replay flow, and results flow.

### Feature 4: Core Cricket Gameplay

Build the ball simulation, batting, bowling, fielding, rules, and match state machine.

### Feature 5: Camera, Input, and Tutorial Flow

Build the camera modes, touch input mapping, controller mapping, and tutorial sequence.

### Feature 6: Mobile MVP and Progression

Build the mobile-first playable version with offline play, short sessions, save/resume, and progression.

### Feature 7: Multiplayer Readiness

Build the network-ready architecture, small multiplayer proof, handoff behavior, and authority model.

### Feature 8: Testing, CI, and Release Hardening

Build automated tests, mobile validation, build pipelines, and production checks.

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
2. Create a feature branch.
3. Implement only that slice.
4. Run tests and Unity validation.
5. Update docs if the design changed.
6. Merge when the exit criteria are met.

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

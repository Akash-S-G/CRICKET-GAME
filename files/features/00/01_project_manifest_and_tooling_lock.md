# Feature 00.1: Project Manifest and Tooling Lock

## Purpose

This doc freezes the project setup so feature work cannot drift on package choices or editor assumptions.

## Locked Choices (free pipeline, no subs - pinned exact per user)

| Package | Version | Source |
|---|---|---|
| Unity Editor | `6000.0.41f1` | `ProjectSettings/ProjectVersion.txt:1` |
| URP | `17.0.3` | `Packages/manifest.json:1` |
| Input System | `1.11.2` | `system_design/input_action_maps.md:6` |
| NGO | `2.4.0` | `TDD.md:5` |
| Addressables | `2.2.2` | `system_design/addressables_grouping.md:16` |
| Animation Rigging | `1.3.0` | `animation_requirements.md:10` |
| Cinemachine | `3.0.1` | `animation_and_scene_pipeline_roadmap.md:318` |
| Timeline | `1.8.6` (from Unity) | `animation_and_scene_pipeline_roadmap.md:74` |
| UGS Auth/CloudSave/RemoteConfig/Analytics/Relay/Lobby | latest 3.x/5.x | `system_design.md:443` |
| Git LFS + Unity MCP | per `system_design/build_pipeline.md:34` | `TECH_STACK.md:5` free tools only: Mixamo/Cascadeur/Plask/MoveAI |

No paid mocap. Animation from free AI + Mixamo `licensing_notes.md:5`.

## Manifest Rules

1. `Packages/manifest.json` must be committed with explicit package entries.
2. `ProjectSettings/ProjectVersion.txt` must be pinned and reviewed in source control.
3. Package upgrades require a docs update before code changes.
4. Only one canonical package choice should exist for each major system.

## Required Repo Files

- `.gitignore`
- `.gitattributes` for LFS patterns
- `Packages/manifest.json`
- `ProjectSettings/ProjectVersion.txt`
- bootstrap scene and service registry

## LFS Tracking

Track:

- `.png`
- `.tga`
- `.psd`
- `.fbx`
- `.anim`
- `.controller`
- `.playable`
- `.wav`
- `.mp3`
- `.mp4`
- `.prefab` only if your project policy treats large prefabs as binary assets

## Validation Checklist

- project opens without package repair
- AI tooling can inspect the editor
- Git LFS is active for large assets
- bootstrap scene loads without package errors

## Exit Criteria

- the Unity setup is frozen enough for feature 00 work
- the manifest is explicit and reviewable
- no agent has to guess package ownership


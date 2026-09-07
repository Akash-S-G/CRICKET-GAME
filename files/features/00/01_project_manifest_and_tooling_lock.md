# Feature 00.1: Project Manifest and Tooling Lock

## Purpose

This doc freezes the project setup so feature work cannot drift on package choices or editor assumptions.

## Locked Choices

- Unity 6 LTS
- URP for runtime rendering
- Input System package
- Addressables package
- Cinemachine package
- Timeline package
- Animation Rigging package
- Netcode for GameObjects package
- Git LFS for binary assets
- Unity MCP connected for AI/editor workflows

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


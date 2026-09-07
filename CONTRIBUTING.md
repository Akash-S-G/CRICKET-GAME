# Contributing Guide

This repo is designed to be developed with AI coding tools, MCP tools, and small reviewable changes.

## Working Rules

- Keep changes small and focused.
- Update docs when design or architecture changes.
- Do not introduce hard-coded mode logic when a data-driven config can do the job.
- Preserve mobile-first behavior unless a doc explicitly says otherwise.
- Keep offline play working even if online systems are not ready.

## AI Tooling Workflow

Recommended stack:

- `Claude Code` for main coding and architecture changes
- `Unity MCP Server` for Unity editor access
- `Cursor` for fast IDE-level iteration
- `Codex CLI` for terminal tasks and repo maintenance
- `Context7 MCP` for current dependency documentation when needed

## Suggested Workflow

1. Read the relevant docs before changing code.
2. Make one feature slice at a time.
3. Validate the change in Unity or with tests.
4. Update the related design doc if the implementation changes the intended behavior.
5. Keep commits small and descriptive.

## Doc Ownership

Use these docs as the main source of truth:

- [files/GDD.md](files/GDD.md)
- [files/system_design.md](files/system_design.md)
- [files/TDD.md](files/TDD.md)
- [files/roadmap.md](files/roadmap.md)
- [files/mobile_roadmap.md](files/mobile_roadmap.md)
- [files/animation_and_scene_pipeline_roadmap.md](files/animation_and_scene_pipeline_roadmap.md)

## Review Checklist

- Does the change fit the mobile-first design?
- Does it keep the game playable offline?
- Does it fit the current milestone?
- Does it respect the animation and scene pipeline?
- Does it remain maintainable by AI tools?


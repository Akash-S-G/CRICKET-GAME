# Cricket Game Design Repo

This repository contains the design, technical, animation, and production docs for a mobile-first cricket game that is planned to support online multiplayer later.

## What’s Here

- Core game design and feature scope
- Technical architecture and system design
- Animation and scene pipeline
- Mobile-first roadmap
- Tutorial and onboarding flow
- Risks, rules, and controller handoff specs
- Data schemas for mode, player, ground, pitch, and physics tuning

## Start Here

1. Read [files/GDD.md](files/GDD.md)
2. Read [files/system_design.md](files/system_design.md)
3. Read [files/development_plan.md](files/development_plan.md)
4. Read [files/animation_and_scene_pipeline_roadmap.md](files/animation_and_scene_pipeline_roadmap.md)
5. Read [files/scene_by_scene_setup.md](files/scene_by_scene_setup.md)
6. Read [files/player_onboarding_and_tutorial_flow.md](files/player_onboarding_and_tutorial_flow.md)

## Design Focus

- Mobile-first gameplay
- AI-assisted development
- Data-driven content
- Offline-first core loop
- Online multiplayer readiness
- Production-ready animation and scene pipeline

## Repo Layout

- `files/` - all current docs and schemas
- `.git/` - git metadata

## Development Notes

- Treat the docs as the source of truth.
- Keep mode logic data-driven.
- Keep multiplayer authoritative on the server when implemented.
- Keep mobile UI, performance, and short sessions as first-class requirements.


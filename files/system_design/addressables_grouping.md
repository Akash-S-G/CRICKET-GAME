# Addressables Grouping

## Purpose

This doc defines how large or optional content is packaged so the base install stays small.

## Group Strategy

### 1. Boot

Always included in the base install.

Contains:

- boot scene assets
- core UI shell
- essential fonts
- loading icons
- minimal audio

### 2. Core Gameplay

Always included.

Contains:

- player prefabs
- ball prefab
- bat prefabs
- pitch and ground essentials
- core match UI
- rule-related presentation assets

### 3. Tutorial

On-demand or delayed download.

Contains:

- tutorial overlays
- tutorial VO
- guided hints
- special practice assets

### 4. Nets

On-demand.

Contains:

- training scene props
- nets-specific background assets
- optional practice VFX

### 5. Stadiums

Remote or on-demand.

Contains:

- stadium meshes
- crowd variants
- environment lighting presets
- replay presentation assets for each stadium

### 6. Cosmetics

Remote or on-demand.

Contains:

- kits
- bats
- accessories
- profile cosmetics

### 7. Audio Optional

Remote if needed.

Contains:

- commentary packs
- seasonal music
- optional ambience variants

## Loading Rules

1. Boot content must always be available locally.
2. Match-critical assets must never depend on a slow remote fetch before play.
3. Optional content may load after the player reaches home or match setup.
4. If remote content is unavailable, the game must fall back to default local content.

## Cache Rules

- Use a bounded cache.
- Prefer last-used content to remain local.
- Evict least-recently-used optional packs first.
- Never evict boot or core gameplay content automatically.

## Delivery Rules

Recommended hosting order:

1. Unity Addressables catalog
2. Unity CCD or equivalent remote hosting
3. Fallback local bundle copy for critical paths

## Budget Guidance

Base install should stay light by keeping non-essential visuals out of the mandatory build.

Track per-group budgets for:

- compressed size,
- load time,
- memory footprint,
- number of variants.

## Dependencies

- Feature 01 content loading
- feature 03 scene flow
- mobile performance budgets

## Tests

- base install loads without optional content
- optional packs resolve when available
- fallback content works when remote delivery fails

## Exit Criteria

- the game starts quickly
- optional content does not bloat the base build
- content grouping is explicit enough to automate


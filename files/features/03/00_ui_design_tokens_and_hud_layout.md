# Feature 03.0: UI Design Tokens and HUD Layout

## Purpose

This doc sets the basic UI system before the scene flow is built in detail.

## Design Tokens

Define these tokens once and reuse them:

- primary background color
- accent color
- warning color
- success color
- text primary
- text secondary
- shadow color
- card radius
- spacing scale
- touch target size

## Typography Rules

- use one readable headline style,
- use one body style,
- keep match HUD text high contrast,
- keep all in-match labels mobile-readable at arm's length.

## HUD Layout Rules

Minimum HUD regions:

- top score strip
- innings and over status
- shot/timing feedback
- ball outcome feedback
- pause and settings
- contextual tutorial prompt

## Mobile Layout Rules

- minimum tap target: `44dp`
- keep critical score text away from screen edges
- keep controls out of the thumb collision zone
- avoid dense overlays during live ball play

## Output Requirements

- one wireframe for home
- one wireframe for match HUD
- one wireframe for tutorial prompt

## Exit Criteria

- UI direction is fixed before scene scaffolding
- the match HUD is legible on mobile
- design tokens are reusable across screens


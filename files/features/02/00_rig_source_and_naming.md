# Feature 02.0: Rig Source and Naming Rules

## Purpose

This doc locks the animation source approach and naming rules so the animation pipeline can be validated automatically.

## Motion Source Decision

Use a hybrid pipeline:

- base locomotion from a retargetable humanoid source,
- cricket-specific actions from bespoke or captured motion,
- procedural correction through Animation Rigging,
- Timeline only for presentation sequences.

## Rig Requirements

- humanoid-compatible skeleton
- stable bone naming across player variants
- consistent root motion handling
- separate bat and hand attachment strategy

## Naming Convention

Use the following pattern:

`[Character]_[Action]_[Variant]_[Timing]`

Examples:

- `Batter_Drive_Early_A`
- `Bowler_Pace_RunUp_A`
- `Fielder_Dive_Left_A`
- `Keeper_Take_Low_A`

## Asset Rules

1. Every animation clip must be named deterministically.
2. Every clip must have a documented purpose.
3. Clips used in gameplay must be referenced in the clip inventory.
4. Retargeting assumptions must be documented for each rig variant.

## Validator Needs

The pipeline should be able to detect:

- missing clip variants,
- duplicate names,
- unassigned animator states,
- incorrect rig mappings,
- unsupported timing labels.

## Exit Criteria

- animation content can be audited by name
- the source approach is not ambiguous
- clip ownership is explicit


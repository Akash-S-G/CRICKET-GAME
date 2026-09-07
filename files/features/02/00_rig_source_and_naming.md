# Feature 02.0: Rig Source and Naming Rules

## Purpose

This doc locks the animation source approach and naming rules so the animation pipeline can be validated automatically.

## Motion Source Decision (locked free/AI, no paid subs per user)

Hybrid free pipeline `animation_requirements.md:10`:
- locomotion/support (~40 clips) from Mixamo (free Adobe, auto-rig) - FBX Humanoid, T-pose.
- signature batting/bowling/keeper (~30 clips) from Cascadeur Community + Blender + AI video-to-FBX (Plask/MoveAI/DeepMotion free tier) self-recorded phone reference, cleaned in Blender, not Rokoko.
- variation/correction via Animation Rigging 1.3.0 Multi-Parent/TwoBoneIK/MultiAim `animation_and_scene_pipeline_roadmap.md:114`.
- Timeline previz via Blender/SD before Timeline polish.
Record every clip `Source`/`License` in `animation_clip_inventory.md:1` + `licensing_notes.md:5` before import.

## Rig Requirements (Unity Humanoid `animation_requirements.md:22`)

- Unity Humanoid avatar `1.8 m`, neutral T-pose, 30k Mid/High 18k Low tris `animation_requirements.md:16`.
- Bones: Hips, Spine, Chest, UpperChest, Neck, Head, Shoulder/Clavicle L/R, UpperArm/LowerArm/Hand, UpperLeg/LowerLeg/Foot/Toe. Sockets: `socket_bat_handle` + `socket_ball_hand` via constraint, never clip offset `animation_requirements.md:22`.
- Root motion: locomotion clips root baked Y, signature bat root locked XZ, validate `start_run`/`pivot` <5cm foot slide `features/02/01_rig_retargeting_and_locomotion.md:16`.
- Fallback: Priority 2 may fallback to Priority 1 on Low `animation_requirements.md:16` `shouldFallback:true` in sidecar; Rig fallback uses Priority 1 reaction if `02/04_fielding_keeper_and_presentation.md:1` clip missing `TDD.md:87`.

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

## Validator Needs (`Assets/_Project/Tools/Editor/ValidateClipNames.cs` + sidecar `animation_events_sidecar.json`)

Checks:
- missing variants per `animation_clip_inventory.md:1` (every `defense/drive/cut` needs `early/good/late` `animation_clip_inventory.md:66`),
- duplicate names `[Character]_[Action]_[Variant]_[Timing]`,
- unassigned Animator state `animation_and_scene_pipeline_roadmap.md:126`,
- rig mapping mismatch Hips/Hand per `animation_requirements.md:22`,
- unsupported `timingQuality` outside `[-1,1]` `GDD.md:178` or non-standard `60/120ms` `delivery_physics_constants.json:20`,
- `Source` empty or `License` not in `{Mixamo-Free, Cascadeur-Community, Plask-Free, MoveAI-Free, DeepMotion-Free, Blender-Custom}` `licensing_notes.md:5`,
- `ContactNorm` outside `0.36-0.44` `animation_requirements.md:60`.
Run via `Window > Cricket > Validate Animation Pipeline` or CI `dotnet` step `system_design/build_pipeline.md:16`.

## Exit Criteria

- animation content can be audited by name
- the source approach is not ambiguous
- clip ownership is explicit


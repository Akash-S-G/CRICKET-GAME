# Asset Catalog - All 3D Assets Needed For Cricket Project (AI Generation Ready)

> For AI agent to generate via `files/FREE_3D_AI_GENERATION_TOOLS.md:1` (TripoSR/TRELLIS/Hunyuan/Blender MCP) with reference image upload. All descriptions are image-to-3D prompts. Priority `P1` must run on Low `1.5M/60` `system_design.md:188` `TDD.md:87` fallback `animation_events_sidecar.json:1`.

## How To Use (Agent + Human)

1. For each asset below, **upload reference image** (T-pose or front photo) to `3D AI Studio` / `TripoSR` / `Hunyuan3D` `files/FREE_3D_AI_GENERATION_TOOLS.md:1`.
2. Prompt is given as `Image + Text` - image drives shape, text drives style/detail. Use `1.8m` Humanoid `features/02/00_rig_source_and_naming.md:16` for characters.
3. Export `FBX 30fps Humanoid` `1.8m` -> `Assets/_Project/.../.keep` tree `unity_ai_workflow_and_project_structure.md:18` (on build machine, docs repo stays docs-only per `AGENT.md:1`).
4. Name `[Character]_[Asset]_[Variant]` per `features/02/00_rig_source_and_naming.md:16` + record `Source/License` `licensing_notes.md:5` + `EventNorm` `animation_events_sidecar.json:1` if animated.
5. Validate `Window > Cricket > Validate Animation Pipeline` `Assets/_Project/Tools/Editor/ValidateClipNames.cs:1`.

---

## A. Characters (Humanoid 1.8m, Rig: Mixamo-Free -> Blender, Source: Plask/MoveAI Free -> Cascadeur per free pipeline `animation_requirements.md:10`)

| ID | Asset | Description For AI (Upload Front T-pose Image + Prompt) | Style | Poly Budget `animation_requirements.md:16` | Variations | Priority | File |
|---|---|---|---|---|---|---|---|
| `CHR_BATSMAN_01` | Batsman Male | `Front T-pose adult male cricketer in white test kit, helmet held in left hand, pads and gloves, athletic build 1.80m, neutral face, bare skin for retarget` | Realistic low-poly `30k` Mid `18k` Low | `30k` `18k` | `batting_stance_front_foot` `bat_drive_*` `animation_clip_inventory.md:56` | P1 | `Assets/_Project/Art/Characters/Batsman.fbx` |
| `CHR_BOWLER_PACE_01` | Pace Bowler Male | `Side T-pose tall fast bowler in white, mid-run stride, arm raised, lean athletic, short hair` | Same | `30k` | `bowl_pace_*` `bowl_runup_pace_long` `animation_clip_inventory.md:153` | P1 | `Art/Characters/BowlerPace.fbx` |
| `CHR_BOWLER_SPIN_01` | Spin Bowler Male | `Front T-pose spinner in white, wrist cocked, compact build, beard` | Same | `30k` | `bowl_offspin_*` `bowl_legspin_*` | P1 | `Art/Characters/BowlerSpin.fbx` |
| `CHR_FIELDER_01` | Fielder Generic | `A-pose generic fielder in white, gloves off, sprinter build, 1.78m` | Same | `30k` | `field_*` `field_dive_*` `animation_clip_inventory.md:203` | P1 | `Art/Characters/Fielder.fbx` |
| `CHR_KEEPER_01` | Wicketkeeper | `Crouch pose keeper in white + gloves + pads, crouch ready, 1.75m` | Same | `30k` | `keeper_*` `keeper_stumping_*` `animation_clip_inventory.md:259` | P2 fallback P1 `TDD.md:87` | `Art/Characters/Keeper.fbx` |
| `CHR_UMPIRE_01` | Umpire | `Front T-pose umpire middle-aged in white coat, hat, neutral, low detail` | Stylized `15k` | `15k` | `umpire_signal_*` `animation_clip_inventory.md:273` | P3 | `Art/Characters/Umpire.fbx` |
| `CHR_CROWD_01` | Crowd Impostor | `Front/back sprite sheet 4x2 crowd varied shirts, low poly billboard` | Sprite `2k` | `2k` | `25% Low 60% Mid 100%` `system_design.md:188` | P1 | `Art/Characters/Crowd.fbx` |

**Character notes:** All share `1.8m` Humanoid skeleton `Hips->Toe` `socket_bat_handle` `animation_requirements.md:22`. Use Mixamo auto-rig -> `Features/02/00_rig_source_and_naming.md:16` fallback `shouldFallback:true` for P2/P3.

---

## B. Props (Static + Attached, Source: TripoSR 6GB <0.5s `VAST-AI-Research/TripoSR:6,933` for fast props)

| ID | Asset | Description For AI (Upload Reference + Prompt) | Dimensions | Style | Poly | Variations | File |
|---|---|---|---|---|---|---|---|
| `PROP_BAT_01` | Cricket Bat | `Close-up front English willow bat, flat face, curved back, brown handle grip, 96cm long, grain visible, studio light` | 96x11x4cm `Prefabs/Bat` `unity_ai_workflow_and_project_structure.md:18` | PBR wood `1024` | `Bat` `socket_bat_handle` attach | `Art/Environment/Bat.fbx` |
| `PROP_BALL_01` | Cricket Ball Red | `Red leather cricket ball, 6 stitch seam, worn shine, 7.2cm diameter` | 7.2cm `Prefabs/Ball` | PBR leather `512` | `Ball` physics `delivery_physics_constants.json:20` COR `0.85` | `Art/Environment/Ball.fbx` |
| `PROP_STUMPS_01` | Stumps + Bails | `Three wooden stumps 71cm + two bails 11cm, light wood, spring-loaded groove, front view` | 71cm `Prefabs/Environment` | Wood `1k` | `stumps` + `bails` physics `StumpBreak 0.52` `animation_events_sidecar.json:1` | `Art/Environment/Stumps.fbx` |
| `PROP_HELMET_01` | Batsman Helmet | `Navy cricket helmet grille front, side view, ABS shell, 58cm` | 28cm | Hard `512` | Helmet | `Art/Characters/Helmet.fbx` |
| `PROP_PADS_GLOVES_01` | Pads + Gloves | `White cricket pads front + batting gloves pair, strap detail` | Pads 60cm | Leather `1k` | Attach to `CHR_BATSMAN` | `Art/Characters/Pads.fbx` |

---

## C. Stadium / Ground / Pitch (Environment, Source: Hunyuan3D 2.1 / TRELLIS 2 `16GB` high detail `cmarix.com`, Modly offline `modly3d.app`)

| ID | Asset | Description For AI (Upload Aerial + Prompt) | Size `ground_schema.json:1` | Style | Poly `system_design.md:188` | Priority | File |
|---|---|---|---|---|---|---|---|
| `GRD_MAIN_01` | Central Stadium Full | `Aerial oval cricket stadium 70m straight 65m cover, 45k capacity, green outfield, central pitch strip, stands 4 sides, floodlights, day` | `straight 70` `cover 65` `point 60` `ground_schema.json:1` `size_class full` | Realistic URP Lit | `80k` | P1 | `Prefabs/Environment/StadiumMain.fbx` Addressable `Stadiums` `system_design/addressables_grouping.md:16` |
| `GRD_GULLY_01` | Gully Small Street | `Small street ground 35m wall, concrete, house backdrop, flat pitch, informal` | `small_gully` `35m` | Stylized `20k` | `20k` | P1 | `Prefabs/Environment/StadiumGully.fbx` |
| `GRD_MIN_01` | MinBoundary Tiny | `Tiny ground 25-35m Close boundary `ground_schema.json:1` `min_boundary_tiny`, arcade walls` | `25m` | `15k` | P2 | `Prefabs/Environment/StadiumMin.fbx` |
| `PITCH_FLAT_01` | Flat Batting Track | `22 yard pitch strip top-flat, green grass stubble, crease white lines 1.22m, flat` | `22yd` `pitch_schema.json:1` `flat` | `5k` | P1 | `Prefabs/Environment/PitchFlat.fbx` |
| `PITCH_GRASSY_01` | Grassy Green Top | `Grassy green pitch `bounce 1.15` `seam 1.3` `pitch_schema.json:1` lush` | - | `5k` | P1 | `Prefabs/Environment/PitchGrassy.fbx` |
| `PITCH_DUSTY_01` | Dry Dusty | `Dry dusty brown cracks, turn `spin 0.5` `pitch_schema.json:1` worn` | `wear 80 overs` | `5k` | P2 `Test` | `Prefabs/Environment/PitchDusty.fbx` |
| `ENV_CROWD_STAND_01` | Stands + Crowd | `Tiered stands with crowd impostors, roof, flags` | - | `25k` Low 25% `system_design.md:188` | P1 | `Art/Environment/Stands.fbx` |
| `ENV_FLOODLIGHT_01` | Floodlights 4x | `Tall floodlight pylon 30m, night use` | 30m | `2k` | P3 night | `Art/Environment/Floodlight.fbx` |
| `ENV_SCOREBOARD_01` | Scoreboard | `LED scoreboard flat, emissive, 10x6m` | 10m | `1k` | P1 | `Prefabs/UI/Scoreboard.fbx` |

---

## D. UI / VFX / Audio (2D + In-Unity)

| ID | Asset | Description For AI (Prompt + Reference) | Size | File |
|---|---|---|---|---|
| `UI_ICON_BAT_01` | Bat Icon | `Flat vector cricket bat icon, white on dark, 256px` | `256` `Art/UI/Icons` | `Art/UI/Icons/Bat.png` `Art/UI` `unity_ai_workflow_and_project_structure.md:18` |
| `UI_SCOREBUG_01` | Score HUD | `Minimal score bug, team 0/0 overs 0.0, dark translucent, 1080p top bar` | `1920x120` | `Prefabs/UI/HUD` `features/03_scene_and_ui_pipeline.md:28` |
| `VFX_TRAIL_01` | Ball Trail | `Red ball trail ribbon, replay `replay_*` `animation_clip_inventory.md:303`` | Particle | `VFX/Trail` `Addressables Replay` |
| `SFX_HIT_01` | Bat Hit | `Willow bat middle hit thwack, edge tick` | `wav` | `Audio/SFX` |

---

## E. Generation Checklist For Agent (Copy For Each Asset)

```
Asset: PROP_BAT_01
Reference: upload front bat photo 1024px (clean background per modly3d.app tip)
Prompt: `Cricket bat English willow flat face 96cm grain studio light PBR`
Tool: TripoSR <0.5s 6GB -> FBX 50K -> Blender decimate 5K URP Lit
Path: Assets/_Project/Art/Environment/Bat.fbx (build machine)
Source: Blender-Custom / TripoSR License: Custom-AI-Generated -> licensing_notes.md:5
Priority: P1 (Low must run) fallback: N/A
Validate: Window > Cricket > Validate (Source allowed per features/02/00_rig_source_and_naming.md:44)
```

**Budget total:** Mixed `96MB Low` `160MB Mid` `240MB High` anim `animation_requirements.md:16` + `120MB base` install `system_design.md:188`. Addressables `Stadiums` remote keeps base `<120MB` `system_design/addressables_grouping.md:16`.

**Style note:** Realistic low-poly URP Lit `30k` chars `1024` props for mobile `GDD.md:40` mobile-first vs `TECH_STACK.md:28` PC legacy - mobile wins.


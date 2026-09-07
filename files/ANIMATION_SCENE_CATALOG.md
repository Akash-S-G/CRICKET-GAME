# Animation Scene Catalog - All Animation Scenes For Cricket Project (AI Generation Ready)

> For AI agent to generate via `files/FREE_3D_AI_GENERATION_TOOLS.md:1` (Plask/Rokoko Vision/MoveAI -> Cascadeur -> Blender) with reference video upload. Each scene below is a 30fps Humanoid FBX `1.8m` `features/02/00_rig_source_and_naming.md:16` with `EventNorm` `animation_requirements.md:60` server `TDD.md:96`. Priority `P1` Low must run `96MB` `animation_requirements.md:16` `TDD.md:87` fallback `animation_events_sidecar.json:1`.

## How To Use (Agent)

1. **Upload reference video** (phone front/side 5s T-pose start) to `Plask` `plask.ai/docs` / `Rokoko Vision` free 15s / `DeepMotion` `files/FREE_3D_AI_GENERATION_TOOLS.md:1`.
2. Download FBX 30fps -> `Blender`/`Cascadeur Community` `AutoPhysics` fix `footSlide <5cm` `features/02/01_rig_retargeting_and_locomotion.md:16` -> export FBX -> `Assets/_Project/Animations/Clips` (build machine, docs repo stays docs-only `AGENT.md:1`).
3. Name `[Character]_[Scene]_[Variant]_[Timing]` per `features/02/00_rig_source_and_naming.md:16`, record `Source` in `{Mixamo-Free, Cascadeur-Community, Plask-Free, MoveAI-Free, DeepMotion-Free, Blender-Custom}` `licensing_notes.md:5`, add row to `files/animation_events_sidecar.json:1`.
4. Validate `Window > Cricket > Validate Animation Pipeline` `Assets/_Project/Tools/Editor/ValidateClipNames.cs:1`.

---

## S1. Batting Scenes (Timing Matrix `60ms good / 120ms early-late` `delivery_physics_constants.json:20` `GDD.md:178`)

| ID | Scene | Cast | Description For AI (Upload Side Video + Prompt) | Duration | Camera `animation_and_scene_pipeline_roadmap.md:318` | EventNorm `animation_requirements.md:60` | Tool | P | File |
|---|---|---|---|---|---|---|---|
| `BAT_IDLE_01` | Stance Idle | Batsman solo | `Batsman T-pose -> stance front-foot lean weight shift breathing 1.8m, pads visible` | Loop 2s | `Batting Broadcast 48 0.30s` | - | Cascadeur | P1 | `Animations/Clips/BatStance.fbx` |
| `BAT_DEF_01` | Defensive Block | Batsman | `Front foot defensive block bat vertical, good timing centered contact, early = outside edge wobble, late = inside edge` x `early/good/late` `animation_clip_inventory.md:66` | 0.8s | `FPP 62 0.20s` | `BatContact 0.40` `0.36-0.44` | Plask->Cascadeur | P1 | `Clips/BatDefensive{fB}.fbx` |
| `BAT_DRIVE_01` | Cover Drive | Batsman | `Front foot cover drive full extension high elbow, good=clean middle, early=outside edge, late=inside` `animation_clip_inventory.md:78` | 0.9s | `Batting 48` | `0.40` | Plask | P1 | `Clips/BatDriveCover{fB}.fbx` |
| `BAT_CUT_01` | Cut | Batsman | `Back foot cut horizontal bat short ball outside off, wristy` `animation_clip_inventory.md:87` | 0.7s | `Batting 48` | `0.39` | MoveAI | P1 | `Clips/BatCut{fB}.fbx` |
| `BAT_PULL_01` | Pull/Hook | Batsman | `Back foot pull swivel hook short 135kph bouncer, power [0,1] `GDD.md:178` medium vs loft` | 0.8s | `Chase 65` | `0.41` | Plask | P1 | `Clips/BatPull{fB}.fbx` |
| `BAT_SWEEP_01` | Sweep/Reverse | Batsman | `Kneeling sweep low, reverse switch, P2 fallback` `animation_clip_inventory.md:108` | 0.9s | `MidWicket 58` | `0.42` | DeepMotion | P2 | `Clips/BatSweep.fbx` |
| `BAT_LOFT_01` | Loft/Six Hit | Batsman | `Loft down ground full power [1], good= sweet COR 0.85 early edge 0.55 `delivery_physics_constants.json:20` | 1.0s | `Chase 65` | `0.38` | Plask | P1 | `Clips/BatLoft.fbx` |
| `BAT_MISS_01` | Mishit | Batsman | `Mishit variants `bat_edge_outside/inside/top_edge` `animation_clip_inventory.md:122` | 0.6s | `FPP 62` | `0.40` | Blender | P1 | `Clips/BatMiss.fbx` |
| `BAT_RUN_01` | Between Wickets | Batsman pair | `Sprint 6m/s call_yes/no/wait turn_at_crease dive_for_crease slide` `animation_clip_inventory.md:132` | Loop 1.5s | `Chase 65 0.20s` | - | Mixamo | P1 | `Clips/BatRun.fbx` |
| `BAT_REACT_01` | Reaction | Batsman | `react_boundary/miss/frustrated` `animation_clip_inventory.md:144` | 1.2s | `Replay 45-60` | - | Blender | P3 | `Clips/BatReact.fbx` |

**Batting rule:** `shotType/timingQuality/shotPower/footworkContext` `GDD.md:178` replicated `TDD.md:41` `PhysicalState.DeliveryId` server wins `TDD.md:96` visual only.

---

## S2. Bowling Scenes (`deliveryType` enum `GDD.md:210` `TDD.md:41` table)

| ID | Scene | Description For AI | Duration | Camera | EventNorm | Tool | P | File |
|---|---|---|---|---|---|---|---|
| `BOWL_RUNUP_PACE_01` | Pace Run-up Long | `Pace 15 step run-up long sprint gather` `animation_clip_inventory.md:153` `bowl_runup_pace_long` | Loop 3s | `Bowling End 52 0.25s` | - | Mixamo | P1 | `Clips/BowlRunupPace.fbx` |
| `BOWL_PACE_STD_01` | Pace Standard | `Pace standard side-on arm high 135kph ReleaseQuality good` `bowl_pace_standard` `BallRelease 0.33` | 1.2s | `Bowling 52` | `0.33` `0.28-0.38` | Plask->Cascadeur | P1 | `Clips/BowlPaceStd.fbx` |
| `BOWL_YORK_01` | Yorker | `Pace yorker full length stump line` | 1.2s | `Bowling 52` | `0.33` | Cascadeur | P1 | `Clips/BowlYorker.fbx` |
| `BOWL_BOUNCE_01` | Bouncer | `Pace bouncer short rising` | 1.2s | `Bowling 52` | `0.33` | Cascadeur | P1 | `Clips/BowlBouncer.fbx` |
| `BOWL_SLOW_01` | Slower Ball | `Pace slower -15kph grip` | 1.2s | `Bowling 52` | `0.33` | Cascadeur | P2 | `Clips/BowlSlower.fbx` |
| `BOWL_OFF_01` | Off Spin | `Off spin short run-up wrist `bowl_offspin_standard`` | 1.1s | `Bowling 52` | `0.32` | MoveAI | P1 | `Clips/BowlOffSpin.fbx` |
| `BOWL_LEG_01` | Leg Spin Googly | `Leg spin googly flipper variation` `bowl_legspin_*` | 1.1s | `Bowling 52` | `0.32` | MoveAI | P1 | `Clips/BowlLegSpin.fbx` |
| `BOWL_FOLLOW_01` | Follow Through | `pace/spin followthrough recover_stance` `animation_clip_inventory.md:187` | 1.0s | `Bowling 52` | - | Cascadeur | P2 | `Clips/BowlFollow.fbx` |

---

## S3. Fielding Scenes (Handoff `handoff_radius 3.5/5.0` `mode_config_schema.json:18` blend `0.15s` `TDD.md:75` `OnHandoffTriggered` `system_design/state_machines_and_event_model.md:33`)

| ID | Scene | Description For AI | Duration | Camera | EventNorm | Tool | P | File |
|---|---|---|---|---|---|---|---|
| `FIELD_IDLE_01` | Ready Stance | `Close_in crouch / outfield ready `field_idle_*` `animation_clip_inventory.md:203` | Loop 1.5s | `TopDown 50 0.35s` | - | Mixamo | P1 | `Clips/FieldIdle.fbx` |
| `FIELD_PICKUP_01` | Pickup Clean | `Clean pickup low/diving slide_stop_left/right `field_pickup_*`` | 0.7s | `Chase 65` | `0.55` `CatchPoint` | Plask | P1 | `Clips/FieldPickup.fbx` |
| `FIELD_THROW_01` | Throw Short/Long | `Short 10m / long 40m / underarm flat relay `field_throw_*`` | 0.8s | `Chase 65` | `ThrowRelease 0.45` `0.35-0.55` | MoveAI | P1 | `Clips/FieldThrow.fbx` |
| `FIELD_CATCH_01` | Catch Variants | `Chest/low/high/diving_forward/side boundary_catch `field_catch_*` `animation_clip_inventory.md:232` | 1.0s | `Chase 65` | `CatchPoint 0.55` `0.45-0.65` | DeepMotion | P1 | `Clips/FieldCatch.fbx` |
| `FIELD_DIVE_01` | Dive Save | `dive_left/right lunge_save cutoff_run `animation_clip_inventory.md:242` | 1.0s | `Chase 65 0.20s` | - | Blender | P1 | `Clips/FieldDive.fbx` |

---

## S4. Keeper + Umpire + Celebration Scenes

| ID | Scene | Description For AI | EventNorm | P | File |
|---|---|---|---|---|---|
| `KEEPER_01` | Keeper Gather | `crouch idle stand_up_spin gather take_edge/low/high `keeper_*` `animation_clip_inventory.md:259` stumping `0.52`` | `StumpBreak 0.52` `0.45-0.60` | P2 `shouldFallback:true` `TDD.md:87` | `Clips/Keeper.fbx` |
| `UMPIRE_01` | Signals | `out/not_out/wide/no_ball/four/six` `umpire_*` `animation_clip_inventory.md:273` | - | P3 | `Clips/Umpire.fbx` |
| `CELEB_01` | Win/Loss | `results_win/loss/celebrate_wicket `animation_clip_inventory.md:284` `290`` | - | P3 Addressable `Stadiums` | `Clips/Celeb.fbx` |

---

## S5. Cinematic / Replay Scenes (Timeline `animation_and_scene_pipeline_roadmap.md:279` `system_design/addressables_grouping.md:16` P3)

| ID | Scene | Description For AI (Previz via Stable Diffusion + Blender) | Duration | Camera | File |
|---|---|---|---|---|---|
| `INTRO_01` | Team Banner Pan | `intro_team_banner match_pan pitch_wide toss_call `animation_clip_inventory.md:284`` | 5s | Timeline `Replay 45-60` `animation_and_scene_pipeline_roadmap.md:318` | `Timelines/Intro.playable` Addressable |
| `REPLAY_01` | Wicket/Boundary Close | `replay_wicket_close batter_swing_close bowler_release_close `animation_clip_inventory.md:303`` | 3s slow 0.5x | Timeline slow | `Timelines/ReplayWicket.playable` |
| `MENU_01` | Menu Ambient | `menu_background_ambient card_hover` `animation_clip_inventory.md:284` | Loop 4s | `Menu` | `Timelines/Menu.playable` |

---

## S6. Environment Scenes (Loop, Low Cost)

| ID | Scene | Description | Poly | File |
|---|---|---|---|---|
| `ENV_CROWD_01` | Crowd Loop | `Crowd wave loop` `ENV_CROWD_STAND_01` `files/ASSET_CATALOG.md:1` 25% Low `system_design.md:188` | 2k | `Animations/Clips/Crowd.fbx` |
| `ENV_BALL_ROLL_01` | Ball Dead | `Ball roll dead after over` | - | `Clips/BallDead.fbx` |

---

## Generation Checklist (Copy Per Scene)

```
Scene: BAT_DRIVE_01 Cover Drive good
Reference: upload side cover drive video 5s (clean)
Prompt: `Front foot cover drive high elbow 1.8m white kit good timing middle contact PBR`
Tool: Plask Free -> Cascadeur AutoPhysics -> Blender Humanoid retarget 30fps
Path: Assets/_Project/Animations/Clips/BatDriveCover_good.fbx (build machine)
Rig: Humanoid 1.8m socket_bat_handle features/02/00_rig_source_and_naming.md:16
Event: BatContact 0.40 0.36-0.44 animation_requirements.md:60 server TDD.md:96
Source: Plask-Free -> Cascadeur-Community -> Blender-Custom License: Custom-AI-Generated -> licensing_notes.md:5
Priority: P1 Low must run fallback: N/A
Validate: Window > Cricket > Validate (Source in allowed set + Event in range)
```

**Budget:** `96MB Low` `160MB Mid` `240MB High` `animation_requirements.md:16` - `P1 40` clips `animation_clip_inventory.md:345` only on Low `TDD.md:87`.


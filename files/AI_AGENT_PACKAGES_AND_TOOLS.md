# AI Agent Packages and Tools - Complete Reference

> Source-of-truth for AI coding agents working on this cricket project. Every package, free tool, and testing stack the agent may use without guessing.
> Free/AI-only pipeline per user (no paid subs). Locked versions per `TDD.md:5` `system_design.md:443` `features/00/01_project_manifest_and_tooling_lock.md:16`.

## 0. How To Use This File

- AI agent must read this before `feat/00` scaffold `feature_implementation_master.md:72`.
- Do not invent a package. If not listed here, propose via docs update `CONTRIBUTING.md:1`.
- All versions pinned. `Packages/manifest.json:1` + `ProjectSettings/ProjectVersion.txt:1` are the install source on build machine (docs repo stays docs-only per user).
- Validation: `Window > Cricket > Validate Animation Pipeline` `Assets/_Project/Tools/Editor/ValidateClipNames.cs:1` + `Unity -runTests` `features/08/00_ci_toolchain_and_commands.md:1`.

---

## 1. 3D Model Tools (Free, Agent Can Generate In <60s)

| Need | Free Tool | Agent Workflow | Output | Ref |
|---|---|---|---|---|
| **Player base mesh** | **Mixamo** `mixamo.com` free Adobe | Upload T-pose `1.8m` -> auto-rig -> download FBX Humanoid | FBX Humanoid `1.8m` `features/02/00_rig_source_and_naming.md:16` | `animation_requirements.md:10` |
| **AI Generate character** | **3D AI Studio** `3daistudio.com` free tier, **Meshy** `meshy.ai` free 200/mo, **Tripo** `tripo3d.ai` | `Text: cricket batsman in whites T-pose` -> Rig -> FBX Mixamo-compatible | FBX + Mixamo rig | `3DAI Studio 2026` test |
| **Stadium / Pitch** | **Sketchfab** CC0 free + **Kenney** `kenney.nl` + **Blender** 4.x | Download `low poly stadium` -> Blender decimate -> URP Lit `30k` Mid `18k` Low `animation_requirements.md:16` `1.5M` `system_design.md:188` | FBX | `unity_ai_workflow_and_project_structure.md:18` |
| **Props (bat, stumps, ball)** | **UModeler X PicoBerry** free Unity Asset Store + **Blender** primitive | In Unity: `UModeler X > Generate 3D: cricket bat low poly` -> `Smart Remesh` `AI Rigging` 1-click | FBX in `Assets/_Project/Prefabs` | `makaka.org` free |
| **Avatar fast** | **Ready Player Me** `readyplayer.me` | Selfie -> `glTF` -> Mixamo rig | `glTF` + FBX | `neolemon 2026` |
| **Cleanup / LOD / Retopo** | **UModeler X Smart Remesh** + **Blender Decimate** | `Remesh` to tier budget `features/06/05_device_tier_budget.md:1` | LOD FBX | `system_design.md:188` |

**Agent rule:** Start `T-pose symmetric 1.8m` - auto-rig 95% vs 40% random pose. Sockets `socket_bat_handle` `socket_ball_hand` `animation_requirements.md:22`.

---

## 2. Animation Tools - Free AI Video-to-Motion (Signature 30 Clips `animation_clip_inventory.md:56`)

| Tool | Cost 2026 | Job | Export to Unity | Free Limit | Ref |
|---|---|---|---|---|---|
| **Mixamo** | $0 | `walk/jog/sprint/pivot/idle/field_idle` 40 support `animation_clip_inventory.md:13` | FBX 30fps Humanoid direct | Unlimited | `toolaipilot 2026` fastest 2h vs 8h Muse |
| **Cascadeur Community** `cascadeur.com` | $0 indie | `batisdefense/coverdrive/bowl_pace` physics `AutoPhysics` `animation_and_scene_pipeline_roadmap.md:114` | FBX/DAE/USD free | Full free | Best paid earned $24 but free covers cricket |
| **Plask** `plask.ai` | Free 3/mo | Phone video `bat_pull` -> FBX | FBX/BVH/GLB | 3 free | Korean indie standard `neolemon` |
| **Rokoko Vision** `vision.rokoko.com` | $0 15s | `bowl_runup` markerless | FBX direct | 15s limit | `neolemon` indie best free |
| **DeepMotion Animate 3D** `deepmotion.com` | Free personal | `field_dive/keeper_take` physics `BatContact 0.36-0.44` `animation_requirements.md:60` | FBX/BVH/GLB | Personal only | `deepmotion Unity` |
| **Move AI Free tier** `move.ai` | $0 limited | `sprint_turn` multi-cam | FBX | Limited | `gamineai 2026` |
| **Blender 4.x** | $0 | `bat_top_edge/miss` hand-key `Rigify` | FBX | Unlimited | `features/02/01_rig_retargeting_and_locomotion.md:16` |

**Steps for `bat_drive_cover_good` signature:**
1. Phone front 1080p 5s T-pose -> `Plask`/`Rokoko Vision` `plask.ai/docs`.
2. Download FBX 30fps -> `Blender`/`Cascadeur` `AutoPhysics` fix `footSlide <5cm` `features/02/01_rig_retargeting_and_locomotion.md:16`.
3. Export FBX T-pose `1.8m` -> `Assets/Animations/Clips` Humanoid `features/02/00_rig_source_and_naming.md:16`.
4. Add `animation_events_sidecar.json:1` `BatContact 0.40` server owns `TDD.md:96`.
5. `Window > Cricket > Validate Animation Pipeline` `Assets/_Project/Tools/Editor/ValidateClipNames.cs:1`.

---

## 3. Rigging Tools (Free, Agent 1-Click)

| Tool | Cost | What agent does |
|---|---|---|
| **Mixamo Auto-Rig** | $0 | Upload mesh -> rig in 60s -> FBX `1.8m` Humanoid `animation_requirements.md:22` bones `Hips->Toe` `features/02/00_rig_source_and_naming.md:16` |
| **AccuRIG** `reallusion.com` | $0 | Single mesh -> rig, pairs Mixamo clips |
| **Blender Rigify / Auto-Rig Pro** | $0 free / $40 one-off | `FBX` `BVH` retarget `features/02/01_rig_retargeting_and_locomotion.md:16` |
| **UModeler X AI Rigging** | $0 in Unity | `AI Rigging` bone + weight 1-click `makaka.org` |

---

## 4. Unity Packages - Core (Locked `6000.0.41f1` `ProjectSettings/ProjectVersion.txt:1`)

| Package ID | Version | Purpose for cricket | Where used | Cost |
|---|---|---|---|---|
| `com.unity.render-pipelines.universal` | `17.0.3` | URP Low/Mid/High 3 assets `system_design.md:188` `1.5M/70` `2.5M/100` `4.0M/120` | `Assets/_Project` URP | Free |
| `com.unity.inputsystem` | `1.11.2` | Action Maps `system_design/input_action_maps.md:16` `40px/120px/150ms` `GDD.md:178` `shotType/timingQuality` | `Features/05/00_action_maps_and_gesture_thresholds.md:1` | Free |
| `com.unity.netcode.gameobjects` | `2.4.0` | Server authoritative `TDD.md:33` `PhysicalState` `TDD.md:58` `30Hz` `TDD.md:36` | `Features/07/04_authority_matrix_and_replication_table.md:1` | Free |
| `com.unity.addressables` | `2.2.2` | `Boot/Core` local, `Stadiums/Cosmetics` CCD `system_design/addressables_grouping.md:16` <120MB base | `IContentService` `system_design/service_interfaces.md:16` | Free |
| `com.unity.services.authentication` | `3.3.0` | `IAuthService` `SignInGuest` `system_design/service_interfaces.md:16` | `Features/01/02_profile_save_session_services.md:1` | Free tier |
| `com.unity.services.cloudsave` | `3.0.0` | `ISaveService` field merge `system_design/save_schema_versioning.md:46` | `Features/01/02` | Free tier |
| `com.unity.services.remote-config` | `3.3.0` | `handoff_radius 3.5` `timing 60/120ms` `system_design/remote_config_and_analytics.md:8` | `Features/01/03_remote_config_and_analytics.md:1` | Free |
| `com.unity.services.analytics` | `5.1.0` | `IAnalyticsService` `delivery_resolved` `system_design/analytics_events_schema.json:1` | `Features/01/03` | Free |
| `com.unity.services.relay` | `2.0.0` | NGO Relay `Features/07/01_authority_and_replication_model.md:1` | Multiplayer | Free tier |
| `com.unity.services.lobby` | `2.0.0` | Lobby `Features/07/02_lobby_and_session_flow.md:1` | Multiplayer | Free |

---

## 5. Unity Packages - Animation (Agent Easy Create)

| Package ID | Version | Purpose | Agent note |
|---|---|---|---|
| `com.unity.animation.rigging` | `1.3.0` | `Rig_Bat_TwoBoneIK` `Head MultiAim` `Feet TwoBoneIK <5cm` `animation_and_scene_pipeline_roadmap.md:114` | Agent `AddComponent<RigBuilder>` 1 line `features/02/05_animator_timeline_and_events.md:1` |
| `com.unity.cinemachine` | `3.0.1` | `FPP 62 0.20s` `Batting 48 0.30s` `animation_and_scene_pipeline_roadmap.md:318` | Cinemachine 3 Free `TDD.md:8` |
| `com.unity.timeline` | `1.8.6` | `intro_*` `replay_*` `animation_clip_inventory.md:13` | From Unity `6000.0.41f1` |
| `com.unity.motionmatching` | `6.2+` | **Best free locomotion 2026** `toolaipilot.com` - `MotionMatching.SetTrajectory` smoother than state machine, needs 12-15 Mixamo clips | Agent must have 15 clips DB `walk/straffe/run/idle/start/stop` |
| `com.unity.ugui` | `2.0.0` | Runtime HUD `44dp` `mobile_roadmap.md:24` `system_design/ui_architecture_and_navigation.md:8` | Free |

---

## 6. Unity Packages - AI Agent Helpers (2026 Best)

| Tool | Package / Repo | How agent uses | Cost | Ref |
|---|---|---|---|---|
| **Official Unity MCP Server** | `com.unity.ai.assistant` `2.17+` `docs.unity3d.com/Packages/com.unity.ai.assistant@2.17/manual/integration/unity-mcp-overview.html` | Controls Editor from IDE `Claude/Cursor` - `Scene/Hierarchy/GameObject/Component/Console` `unity.com/blog/unity-ai-mcp-how-to-get-started` | Free, no credits | `TECH_STACK.md:5` |
| **IvanMurzak Unity-MCP** | `github.com/IvanMurzak/Unity-MCP` 71 tools | Community backup `Claude highly recommended` `TECH_STACK.md:5` | MIT | Fallback |
| **AnkleBreaker unity-mcp-server** | `github.com/AnkleBreaker-Studio/unity-mcp-server` 268 tools 30 cats | Most comprehensive - `Scene/Physics/Terrain/ShaderGraph/Profiling/Animation` | Open license | 406 stars `2026-02` |
| **IvanMurzak Unity-AI-Animation** | `github.com/IvanMurzak/Unity-AI-Animation` `com.ivanmurzak.unity.mcp.animation` | MCP for `Animator Controller/AnimationClip` AI create/edit | MIT | `112 stars` |
| **Unity AI Assistant** | `com.unity.ai.assistant` | In-editor chat `Muse` `unity.com/features/ai` - generates skybox/materials (not for timing `toolaipilot`) | $10/mo Personal free 14d | Pro includes |
| **UModeler X PicoBerry** | Asset Store free | `Generate 3D: cricket bat` 60s + `Smart Remesh` + `AI Texturing` in Unity | Free tier | `makaka.org` |

**Install MCP (build machine):** `Unity -> Package Manager -> Add via git URL -> com.unity.ai.assistant` then `Window > Unity MCP Server` enable `TECH_STACK.md:5` `development_plan.md:26`.

---

## 7. Testing Packages and Tools

### 7.1 Unity Testing Stack (Locked)

| Package ID | Version | Purpose | Command |
|---|---|---|---|
| `com.unity.test-runner` | `1.3.0` | Test framework backend | `Unity -runTests` |
| `com.unity.test-framework` | `1.3.0` | `NUnit` `EditMode` `PlayMode` `features/08/01_edit_and_play_mode_tests.md:1` | `Tests/EditMode/SchemaValidationTests.cs:1` |
| `com.unity.test-framework.performance` | `3.0.0` (optional) | `features/08/02_mobile_validation_and_performance.md:1` `95% frames stable` `features/06/05_device_tier_budget.md:1` | `PerformanceTest` |
| `com.unity.ide.visualstudio` | `2.0.22` | VS debug `TECH_STACK.md:5` | Free |

### 7.2 Test Layers (Per `features/08/00_ci_toolchain_and_commands.md:1` `system_design/build_pipeline.md:16`)

| Layer | Where | What | Tool | CI |
|---|---|---|---|---|
| **EditMode** | `Tests/EditMode/` | `SchemaValidationTests.cs:1` `mode_config_schema.json:1` + sidecar `animation_events_sidecar.json:1` + `ProfileDataV2->V3` `system_design/save_schema_versioning.md:45` | `Unity -runTests -testMode EditMode` `features/08/01_edit_and_play_mode_tests.md:1` | `ci.yml` `game-ci/unity-test-runner@v4` `EditMode` |
| **PlayMode** | `Tests/PlayMode/` | `Boot->Home->Nets` `system_design/state_machines_and_event_model.md:16` `Handoff 0.15s` `TDD.md:75` `Ball 30Hz` `system_design/physics_tick_and_reconciliation.md:16` | `Unity -runTests -testMode PlayMode` | `ci.yml` `PlayMode` |
| **Mobile Validation** | `features/08/02_mobile_validation_and_performance.md:1` | `Tier` `Adreno 610 30fps 1.5M/60` `system_design.md:188` vs `730 45fps 2.5M` vs `865 60fps 4.0M` `features/06/05_device_tier_budget.md:1` | Unity Profiler `Memory Profiler` `Frame Debugger` | `AnkleBreaker MCP Profiling` 268 tools |
| **Schema CLI** | `files/*.json` | `ajv validate -s schema -d data` `features/08/00` | `npm i -g ajv-cli` | `ci.yml` validate step |

### 7.3 CI and Validation Tools (Free)

| Tool | Ver | Why | File |
|---|---|---|---|
| **GameCI** `game-ci/unity-builder@v4` + `unity-test-runner@v4` | `v4` | Build Android + dedServer `6000.0.41f1` `system_design/build_pipeline.md:8` | `.github/workflows/ci.yml:1` (on build machine) |
| **ajv-cli** `npm i -g ajv-cli` | `5.x` | Validate `camera_config_schema.json:1` `progression_reward_schema.json:1` `tutorial_step_schema.json:1` | `features/08/00_ci_toolchain_and_commands.md:1` |
| **ValidateClipNames.cs** | `1` | Check `Source` allowed `Mixamo-Free/...` + `BatContact 0.36-0.44` `animation_requirements.md:60` + duplicate | `Assets/_Project/Tools/Editor/ValidateClipNames.cs:1` (doc ref `features/02/00_rig_source_and_naming.md:44`) |
| **Unity Profiler + Frame Debugger + Memory Profiler** | `1.0` | `boot time` `scene load` `save_fail` `system_design/observability_stack.md:24` | `Window > Analysis` |
| **UGS Diagnostics (Crash)** | latest | `crashes/ANR` `system_design/security_threat_model.md:16` `crashReporting` | `system_design/observability_stack.md:16` |

**Commands (build machine, docs repo stays docs-only per user):**
```bash
# EditMode
Unity -batchmode -projectPath . -runTests -testMode EditMode -testFilter SchemaValidation -logFile -
# PlayMode
Unity -batchmode -projectPath . -runTests -testMode PlayMode -logFile -
# Schema CLI
npm install -g ajv-cli
ajv validate -s files/mode_config_schema.json -d files/mode_config_schema.json --strict=false
ajv validate -s files/camera_config_schema.json -d files/camera_config_schema.json --strict=false
# Animation
Unity -batchmode -executeMethod ValidateClipNames.ValidateAndExit -logFile -
```

---

## 8. Full Install Checklist (Build Machine Only)

```bash
# 1. Pin Unity
# ProjectSettings/ProjectVersion.txt -> 6000.0.41f1 per features/00/01_project_manifest_and_tooling_lock.md:16
# 2. Packages/manifest.json add deps §4-5-7.1
# 3. LFS
git lfs install
# .gitattributes already lists *.png *.fbx *.unity filter=lfs
# 4. Create tree
mkdir -p Assets/_Project/{Art,Animations/Prefabs,Scenes/Boot,Scripts/Core,ScriptableObjects/Modes,Tests/EditMode}
# 5. MCP
# Unity: Window > Package Manager > Add via git -> com.unity.ai.assistant
# IDE: claude --mcp-add unity -- npx -y unity-mcp-server
# 6. Verify
# Window > Cricket > Validate Animation Pipeline -> PASS per animation_requirements.md:60
# Unity -runTests -testMode EditMode -> PASS 3 tests Tests/EditMode/SchemaValidationTests.cs:1
```

## 9. References

- `TDD.md:5` `TECH_STACK.md:5` `system_design.md:443` pinned `6000.0.41f1` `URP 17.0.3`
- `animation_requirements.md:10` hybrid free: `Cascadeur/Blender + Plask/MoveAI/DeepMotion Free + Mixamo`
- `animation_clip_inventory.md:1` 130 clips `P1 40` `TDD.md:87` Low
- `animation_events_sidecar.json:1` `BatContact 0.40` `BallRelease 0.33` server `TDD.md:96`
- `features/02/00_rig_source_and_naming.md:44` `ValidateClipNames.cs` `Source` allowed set
- `system_design/build_pipeline.md:8` `GameCI v4` `ajv` `6000.0.41f1`
- Web: `toolaipilot.com 2026-06-20` Mixamo best free, `AnkleBreaker 268` `IvanMurzak 71` MCP, `UModeler X PicoBerry` `makaka.org`, `3DAI/Move/DeepMotion` free tiers.


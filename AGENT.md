# AGENT BUILD GUIDE - How To Access Docs, Build In Order, Maintain Docs, and Avoid Hallucination

> **For AI coding agents (Claude Code / Cursor / MCP).** This repo is docs-only planning per user (`Assets/` not tracked here - built on other machine). Treat `files/` as source-of-truth `README.md:13`. Follow order exactly or you will drift `risks.md:21`.

## 0. Where The Truth Lives (Do Not Invent)

| What | File | When to read |
|---|---|---|
| **Start here** | `files/INDEX.md:1` | First. Lists every doc |
| **Game rules** | `files/GDD.md:1` `GDD.md:178` `GDD.md:210` | Before any gameplay |
| **Tech contract** | `files/TDD.md:5` `TDD.md:47` `TDD.md:58` `TDD.md:87` | Before any code |
| **Architecture** | `files/system_design.md:1` + `files/system_design/INDEX.md:1` 11 subdocs (services `system_design/service_interfaces.md:16` save `system_design/save_schema_versioning.md:16` input `system_design/input_action_maps.md:16` etc.) | Before `feat/00` |
| **Build order** | `files/feature_implementation_master.md:72` (00->08) + `files/milestone_checklist.md:5` | Governs branch order |
| **Packages/tools** | `files/AI_AGENT_PACKAGES_AND_TOOLS.md:1` + `files/TECH_STACK.md:5` | Before scaffold |
| **Animation** | `files/animation_requirements.md:10` `files/animation_clip_inventory.md:1` `files/animation_events_sidecar.json:1` `files/animation_and_scene_pipeline_roadmap.md:114` `features/02/00_rig_source_and_naming.md:16` | Before import |
| **Schemas** | `files/mode_config_schema.json:1` `files/player_schema.json:1` `files/camera_config_schema.json:1` `files/progression_reward_schema.json:1` `files/tutorial_step_schema.json:1` | Before data layer |

**Never read a feature doc without its dependencies:** `feature_implementation_master.md:72` 01 needs 00, 04 needs 01+02+03, 07 needs 04+06.

## 1. Required Read Order Before Writing Any Code

```
1. README.md + CONTRIBUTING.md:1
2. files/INDEX.md:1
3. files/GDD.md:1 (focus GDD.md:178 shot contract + GDD.md:210 bowling table)
4. files/TDD.md:1 (focus TDD.md:5 pins 6000.0.41f1 + TDD.md:47 IPlayerController + TDD.md:58 PhysicalState + TDD.md:87 tiers)
5. files/system_design.md:1 top then files/system_design/INDEX.md:1 read order 1-5 (service_interfaces -> save -> input -> addressables -> remote_config)
6. files/AI_AGENT_PACKAGES_AND_TOOLS.md:1 (free pipeline, no subs)
7. files/feature_implementation_master.md:72 + the exact feature you will implement (e.g. features/00_foundation_and_tooling.md:1 + features/00/01_project_manifest_and_tooling_lock.md:16)
```

**If you skip 1-5 you will hallucinate** versions (`6000.0.41f1` vs `6000.0.x`), `Source` tags (`Mixamo-Free` vs `Rokoko`), or `handoff_radius` (`3.5` vs invent).

## 2. Build Order You Must Follow (Do Not Reorder)

```
feature_implementation_master.md:72 order:
feat/00-foundation        -> Project manifest 6000.0.41f1 + Assets/_Project tree unity_ai_workflow_and_project_structure.md:18 + ServiceRegistry system_design/service_interfaces.md:16
  feat/01-data-services     -> 01/01 schemas, 01/02 ISaveService, 01/03 RemoteConfig/Analytics files/AI_AGENT_PACKAGES_AND_TOOLS.md:1 §7
    feat/02-animation       -> Rig 1.8m Humanoid animation_requirements.md:22 + sidecar files/animation_events_sidecar.json:1 + ValidateClipNames.cs features/02/00_rig_source_and_naming.md:44
    feat/03-scenes-ui       -> 11 scenes scene_by_scene_setup.md:14 + uGUI stack system_design/ui_architecture_and_navigation.md:16 + Cinemachine 3.0.1
      feat/04-gameplay      -> Ball 30Hz system_design/physics_tick_and_reconciliation.md:16 + timing 60/120ms delivery_physics_constants.json:20 + handoff 0.15s TDD.md:75
        feat/05-camera-input-> Action Maps system_design/input_action_maps.md:16 + FOV table animation_and_scene_pipeline_roadmap.md:318 + tutorial files/tutorial_step_schema.json:1
          feat/06-mobile     -> Tier budgets system_design.md:188 Adreno 610 1.5M/60 + progression files/progression_reward_schema.json:1 non-P2W GDD.md:214
            feat/07-multiplayer -> Authority matrix features/07/04_authority_matrix_and_replication_table.md:1 NGO 2.4.0 TDD.md:5 interest TDD.md:39
              feat/08-testing   -> Tests/EditMode/SchemaValidationTests.cs:1 GameCI ci.yml features/08/00_ci_toolchain_and_commands.md:1
```

Branch: `feat/00-foundation` ... `feat/08-testing` `feature_implementation_master.md:30` tag `v0.2` `v0.3-nets` `feature_implementation_master.md:40`.

**Rule:** Do not start `feat/04` without `01`+`02`+`03` done `feature_implementation_master.md:72` (ball needs `ISaveService` + rig + `UIRoot`). CI fails `system_design/build_pipeline.md:16` if violated.

## 3. How To Maintain Docs and Track Work (Todo in MD)

**Agent must keep a `TASKS.md` in repo root (create if missing) and edit it every slice:**

```markdown
# TASKS - Agent Todo (updated 2026-xx-xx)
> Single source of progress. Edit this file, do not keep separate memory.

## Current Milestone
- [ ] feat/00-foundation - Branch `feat/00-foundation` - Owner: agent
  - [ ] Pin manifest 6000.0.41f1 `TDD.md:5` `Packages/manifest.json:1`
  - [ ] Validate sidecar `files/animation_events_sidecar.json:1` BatContact 0.36-0.44 `animation_requirements.md:60`

## Blocked by Docs
- [ ] A2: Add BatContact per-clip in sidecar (needs video)

## Completed
- [x] GDD shot contract GDD.md:178

## Doc Drift Log
- 2026-xx-xx: Changed URP 17.0.3 -> 17.0.4 in manifest + updated TDD.md:5 + feature_implementation_master.md:72
```

**Workflow per slice:**
1. Read feature doc + dependencies (§1 read order).
2. Create branch `feat/XX` `feature_implementation_master.md:30`.
3. Update `TASKS.md` - add todo, mark `in_progress`.
4. Implement only that slice (one scene/service/controller `unity_ai_workflow_and_project_structure.md:68`).
5. `Unity -runTests -testMode EditMode` `features/08/00_ci_toolchain_and_commands.md:1` + `Validate Animation Pipeline`.
6. If behavior changed, update the **docs first** `CONTRIBUTING.md:1` `feature_implementation_master.md:72` Doc Drift Log in `TASKS.md`, then code.
7. `rtk git add` docs + code, merge only on `milestone_checklist.md:5` exit criteria pass.

**Docs as code:** Every `features/0*` subdoc `feature_implementation_master.md:72` is a contract - treat `01/01_data_contracts_and_schemas.md:16` `schemaVersion` change as breaking.

## 4. Hallucination / Misdirection Guard - Check Before Every Commit

Run this checklist or you will mislead the next agent:

- [ ] **Versions pinned?** `TDD.md:5` `6000.0.41f1` not `6000.0.x`, `17.0.3/2.4.0/1.3.0/3.0.1` not `1.2.x`. Search `TDD.md` vs `features/00/01_project_manifest_and_tooling_lock.md:16` mismatch.
- [ ] **Folder tree locked?** `Assets/_Project/...` `unity_ai_workflow_and_project_structure.md:18` not `Assets/Core` `TDD.md:105` legacy alias. Fail CI if script outside `Assets/_Project` `features/00_foundation_and_tooling.md:16`.
- [ ] **Source tags free only?** `Source` must be in `{Mixamo-Free, Cascadeur-Community, Plask-Free, MoveAI-Free, DeepMotion-Free, Blender-Custom}` `features/02/00_rig_source_and_naming.md:44` `licensing_notes.md:5`. No `Rokoko` per user free rule `animation_requirements.md:10`.
- [ ] **Timing single truth?** `60ms good / 120ms early-late` `delivery_physics_constants.json:20` only. No second table in `animation_requirements.md:60` or `GDD.md:178`.
- [ ] **Authority not duplicated?** Server owns `DeliveryId/ServerTick` `TDD.md:58` `system_design/physics_tick_and_reconciliation.md:16` `30Hz`, client only `AnimationStateId` visual `TDD.md:96`. No client hit resolve.
- [ ] **Schemas exist?** Every `ScriptableObjects/Cameras` needs `files/camera_config_schema.json:1` etc. `ajv validate` `features/08/00_ci_toolchain_and_commands.md:1` must pass `Tests/EditMode/SchemaValidationTests.cs:1`.
- [ ] **No invented fields?** `profile.json` fields from `features/01/00_profile_field_list_and_versions.md:1` `schemaVersion/dataVersion` `system_design/save_schema_versioning.md:33` only. No `currencyHard` unless `GDD.md:214` non-P2W allows.
- [ ] **Stubs filled?** `features/00/01` `01/00` `03/00` `06/04` `06/05` `07/00` were templates - check they now have tables `features/06/04_progression_numbers_and_unlock_table.md:1` `xpForNextLevel` `features/06/05_device_tier_budget.md:1` `1.5M/60`.
- [ ] **Refs resolve?** Every `system_design/...` ref `system_design/INDEX.md:1` exists on disk `files/system_design/*.md` + `files/animation_events_sidecar.json:1`. No `rm -rf` needed (docs-only repo).
- [ ] **Docs updated?** If code changed `TDD.md:41` `IPlayerController` or `GDD.md:178` contract, docs commit before code `CONTRIBUTING.md:1`.

**Common hallucination traps in current docs:**
- `TECH_STACK.md:28` says Main Mode PC-first vs `system_design.md:32` mobile-first - **mobile-first wins** `GDD.md:40` `TECH_STACK.md:28` note is legacy. Don't prioritize PC shadows over `system_design.md:188` Low 30fps.
- `features/02` subdocs say `hybrid` without tool - **free hybrid** `animation_requirements.md:10` Cascadeur+Plask+Mixamo wins, not Rokoko.
- `TDD.md:105` `Assets/Core` vs `_Project` - **_Project wins** `unity_ai_workflow_and_project_structure.md:18`.

## 5. Quick Start for New Agent Session

```bash
rtk ls files/ | head
rtk read files/INDEX.md
rtk read files/AI_AGENT_PACKAGES_AND_TOOLS.md # §4-7 versions
rtk read files/feature_implementation_master.md | head -n 60
rtk read TASKS.md # your todo
rtk git status --porcelain # M files/*.md = docs drift, ?? = new schema
# Then pick next feat per §2 order and read its feature doc + dependencies
```

Update `TASKS.md` every 30 min, commit docs + code together `feature_implementation_master.md:30` one slice per PR.


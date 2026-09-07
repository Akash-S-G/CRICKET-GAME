# Animation and Scene Pipeline Roadmap

This document explains how to animate the players, how to build the scenes, and how to use AI coding tools and MCP tools to get to a production-ready result.

It is written for a mobile-first cricket game that will later support online multiplayer.

## 1. Goal

The animation and scene pipeline should make the game feel:

- cricket-specific,
- responsive,
- readable on mobile,
- strong enough for broadcast-style presentation later,
- scalable for multiplayer and live updates,
- maintainable by AI coding tools.

The pipeline should not be a one-off animation setup. It should be a reusable production system.

## 2. What Needs to Be Animated

### 2.1 Player Animation

Core player animation categories:

- batting stance and ready pose
- shot preparation
- shot execution by shot type
- batting timing variations
- running between wickets
- turning, stopping, diving, and sliding
- bowling run-up
- bowling delivery actions by type
- bowling follow-through
- fielding idle and ready poses
- pickup, throw, catch, dive, and relay actions
- wicketkeeper crouch, gather, and stumping
- umpire signals
- celebrations, frustration, and reaction states

### 2.2 Scene Animation

Game scenes also need motion:

- menu background motion
- stadium crowd motion
- camera transitions
- match start presentation
- over changes
- wicket cut-ins
- score and result transitions
- replay sequences

### 2.3 Environment Animation

Environment motion should include:

- crowd loops,
- scoreboard changes,
- lighting changes for time-of-day if used,
- ambient props,
- boundary signage,
- UI motion and screen transitions.

## 3. Recommended Animation Stack

### 3.1 Core Unity Tools

- `Animator Controller` for animation state machines.
- `Animation Rigging 1.2.x` for IK, aiming, and procedural adjustments.
- `Cinemachine 3.0.x` for dynamic and mode-based camera behavior.
- `Timeline` for match intros, replays, wickets, and menu presentation.
- `Addressables` for loading animation-heavy or cinematic content on demand.

### 3.2 AI Tools

- `Claude Code` for code generation, animation-state wiring, and scene logic.
- `Unity MCP Server` for direct scene, object, and component editing from the editor.
- `Cursor` for fast script and scene iteration.
- `Codex CLI` for batch maintenance, validation, and repo operations.
- `Context7 MCP` for up-to-date docs when working with newer APIs or packages.

### 3.3 External Animation Resources

- `Rokoko` for motion capture and AI-assisted mocap workflows.
- `Mixamo` for fast auto-rigging and base animation clips.
- `Adobe Firefly` for concept visuals, quick motion concepts, and asset ideation.

## 4. Production Animation Strategy

Use a hybrid pipeline:

1. Mocap or strong base clips for cricket-defining actions.
2. Marketplace or Mixamo animations for support motion.
3. Animation Rigging for procedural correction and variation.
4. Timeline for cinematic sequences.
5. AI tools for generation, cleanup, and iteration.

This avoids the common trap of having generic animation everywhere.

## 5. Player Rig Pipeline

### 5.1 Rig Standard

All player characters use Unity Humanoid, a neutral T-pose, a `1.8 m` reference height, and the same Unity Human Bone mapping. The required skeleton and prop sockets are locked in `animation_requirements.md` and `features/02/00_rig_source_and_naming.md`.

### 5.2 Rigging Rules

- The rig must support batting, bowling, fielding, running, and wicketkeeper poses.
- Hands, shoulders, spine, hips, knees, ankles, and head must be stable in retargeting.
- Props like bats and ball-handling should be controlled through constraints, not manual offsets.
- Use an upper-body avatar mask for batting, bowling, and throwing layers so locomotion remains stable underneath action motion.

### 5.3 Animation Rigging Use Cases

Use `Animation Rigging` for:

- bat alignment to ball contact using a Multi-Parent or Two-Bone IK setup,
- bowling arm correction using Two-Bone IK with an elbow hint,
- head and eye targeting using Multi-Aim constraints,
- foot planting using Two-Bone IK with ground probes,
- wicketkeeper crouch adaptation using Two-Bone IK,
- fielding aim and throw direction using Multi-Aim plus hand IK,
- procedural cleanup of retargeted motion without changing authoritative gameplay state.

## 6. Animation State Architecture

### 6.1 Animator Layers

Use a layered Animator Controller:

- Base locomotion layer
- Batting action layer
- Bowling action layer
- Fielding action layer
- Keeper layer
- Reaction layer
- Additive polish layer

### 6.2 Key Parameters

Recommended parameters:

- `isBatting`
- `isBowling`
- `isFielding`
- `isWicketKeeper`
- `movementSpeed`
- `shotType`
- `timingQuality`
- `shotPower`
- `footworkContext`
- `deliveryType`
- `releaseQuality`
- `diveTrigger`
- `throwPower`
- `isCelebrating`
- `isReacting`

Parameter contract:

| Parameter | Type/range | Source |
|---|---|---|
| `shotType` | enum | resolved delivery intent |
| `shotDirectionDeg` | float `[-180, 180]` | stick/swipe intent |
| `timingQuality` | float `[-1, 1]` | physics timing window |
| `shotPower` | float `[0, 1]` | normalized hold/swipe |
| `footworkContext` | enum | stick or delivery-length automation |
| `deliveryType` | enum | bowling intent |
| `releaseQuality` | float `[-1, 1]` | bowling release timing |

The `60 ms` good and `120 ms` early/late timing windows come from `delivery_physics_constants.json`. Animation does not maintain a second timing table.

### 6.3 State Machine Rules

- Do not overuse layers that are always evaluated.
- Keep transitions readable and predictable.
- Avoid rebind-heavy runtime changes unless absolutely necessary.
- Keep generic locomotion stable and let action layers override the pose.

## 7. Cricket Animation Matrix

### 7.1 Batting Matrix

Create animation clips for:

- defensive block
- straight drive
- cover drive
- on drive
- cut
- pull
- hook
- sweep
- loft
- reverse sweep
- paddle / improvised shots if in scope

Each shot should have timing variants:

- early
- good
- late

Each gameplay clip also records `shotPower` on the `[0, 1]` blend axis and a normalized `BatContact` event. The event must fall between `0.36` and `0.44`; physics remains authoritative for the actual contact result.

### 7.2 Bowling Matrix

Create delivery variants for:

- pace
- yorker-length pace
- seam
- off-spin
- leg-spin
- slower ball
- variation deliveries if needed

Each delivery should support:

- run-up
- release
- follow-through
- mild execution-error variants

### 7.3 Fielding Matrix

Create fielding clips for:

- ready stance
- sprint
- pickup
- slide stop
- throw short
- throw long
- catch chest height
- catch low
- catch diving
- boundary save
- direct hit throw

### 7.4 Keeper Matrix

Create keeper clips for:

- crouch idle
- gather standing up to the stumps
- stump attempt
- catch behind wicket
- appeal

## 8. Scene Architecture

### 8.1 Required Scenes

The app should have a clear scene breakdown:

- Boot / Splash
- Login / Profile
- Home / Main Menu
- Tutorial / Onboarding
- Practice / Nets
- Match Setup
- Gameplay Match Scene
- Replay Scene or Replay Overlay
- Results / Rewards
- Settings / Customize
- Loading / Transition scene if needed

### 8.2 Scene Responsibilities

Each scene should do one job:

- Boot handles initialization.
- Home handles navigation.
- Tutorial handles learning.
- Nets handles safe practice.
- Match handles simulation and presentation.
- Replay handles cinematic review.
- Results handles reward delivery.

Do not make one scene do everything.

### 8.3 Scene Loading Strategy

Use async loading and transition states so the game feels fast on mobile.

Recommended approach:

- lightweight bootstrap scene,
- additive loading for large gameplay scenes,
- Addressables for heavy or optional assets,
- cached content for frequently used scenes.

## 9. Camera and Scene Presentation

Use camera modes as part of the scene design, not just a gameplay toggle.

Recommended camera modes:

- `FPP / Batter View`
- `Batting Broadcast Camera`
- `Bowling End Camera`
- `Mid-Wicket Tactical Camera`
- `Chase Camera`
- `Top-Down Tactical Camera`
- `Replay Camera`

Use `Cinemachine` for:

- camera blending,
- framing,
- target following,
- cut-ins,
- replay shots,
- match intro shots.

Camera contract:

| Mode | FOV | Blend/cut |
|---|---:|---:|
| FPP / Batter View | 62 degrees | 0.20 s |
| Batting Broadcast | 48 degrees | 0.30 s |
| Bowling End | 52 degrees | 0.25 s |
| Mid-Wicket Tactical | 58 degrees | 0.30 s |
| Chase | 65 degrees | 0.20 s |
| Top-Down Tactical | 50 degrees | 0.35 s |
| Replay | 45-60 degrees | Timeline-controlled |

Camera values are presentation settings. Camera blends must not delay or modify the input timing window.

Use `Timeline` for:

- match intro presentation,
- wicket celebration shots,
- replay packages,
- menu ambient motion,
- cutscene-like transitions.

## 10. AI-Based Production Roadmap

### Phase 0: Reference and Planning

- Collect reference clips for cricket batting, bowling, fielding, keeper work, and stadium presentation.
- Define the animation matrix and the scene list.
- Map each animation need to a source: mocap, Mixamo, AI-generated concept, or procedural rigging.
- Set up Unity MCP and editor automation.

### Phase 1: Rig and Locomotion

- Build the base player rig.
- Import or create idle, walk, jog, sprint, and turn clips.
- Set up Animator Controllers and blend trees.
- Verify retargeting across all player models.

### Phase 2: Cricket Core Actions

- Implement batting clips and timing variants.
- Implement bowling actions and follow-throughs.
- Implement fielding pickup, throw, catch, and keeper basics.
- Add Animation Rigging for bats, hands, head, and foot placement.

### Phase 3: Scene and Camera System

- Build boot, home, tutorial, Nets, and match scenes.
- Set up Cinemachine cameras and default camera rules.
- Add Timeline sequences for intros, wickets, and replays.
- Add animated menu backgrounds and lightweight ambience.

### Phase 4: Gameplay Feedback and Polish

- Add timing feedback to batting.
- Add release-quality feedback to bowling.
- Add fielding handoff blending.
- Add reaction, celebration, and failure states.
- Tune animation transitions and root motion.

### Phase 5: Mobile Optimization

- Reduce clip memory footprint.
- Use lower-cost crowd and UI motion on low-end devices.
- Keep animation state machines lean.
- Avoid expensive runtime rebinding.
- Profile scene transitions and camera cuts.

### Phase 6: Multiplayer Readiness

- Make animation state updates deterministic enough for network replication.
- Ensure handoffs look the same on server and clients.
- Test latency and ownership transitions.
- Separate presentation-only animation from gameplay-critical state.

### Phase 7: Live Content and Expansion

- Add seasonal cosmetics, stadium variants, and special presentations.
- Load optional cinematic assets with Addressables.
- Add new animation sets without changing core game logic.

## 11. AI Workflow for Animation Production

### 11.1 What AI Should Do

- generate boilerplate animation state code,
- wire Animator parameters,
- create scene bootstrap code,
- generate Timeline helper scripts,
- build editor tooling,
- write validation scripts,
- generate asset manifests,
- create first-pass descriptions for mocap or animation assets.

### 11.2 What AI Should Not Do Alone

- final animation quality decisions,
- performance-critical gameplay logic,
- hand-authored cricket motion polish,
- core rig constraints without review,
- shipping camera logic without testing.

### 11.3 Best AI Workflow

1. Define the motion requirement in text.
2. Generate a reference list and clip map.
3. Create or import the base clip.
4. Retarget and test in Unity.
5. Add rigging and blend correction.
6. Validate against gameplay timing.
7. Iterate until the motion feels cricket-specific.

## 12. Production Acceptance Criteria

The animation and scene pipeline is good enough when:

- batting, bowling, and fielding all have readable state changes,
- the player can tell good timing from bad timing,
- scene transitions are fast on mobile,
- camera modes feel intentional and useful,
- the game can load, play, and return to menu without instability,
- animation assets can be added without major rework,
- multiplayer handoff and presentation are visually stable.

## 13. Recommended Resources

### Unity Docs

- [Animator Controller](https://docs.unity3d.com/6000.5/Documentation/Manual/class-AnimatorController.html)
- [Animator Controller asset](https://docs.unity3d.com/6000.5/Documentation/Manual/Animator.html)
- [Animation Rigging package](https://docs.unity3d.com/Packages/com.unity.animation.rigging%401.2/manual/index.html)
- [Animation Rigging workflow](https://docs.unity3d.com/Packages/com.unity.animation.rigging%401.2/manual/RiggingWorkflow.html)
- [Cinemachine 3](https://docs.unity3d.com/Packages/com.unity.cinemachine%403.0/manual/index.html)
- [Cinemachine package overview](https://docs.unity3d.com/Packages/com.unity.cinemachine%40latest/)
- [Timeline and VFX integration](https://docs.unity3d.com/Packages/com.unity.visualeffectgraph%4016.0/manual/Timeline.html)
- [Unity AI tools](https://docs.unity.com/en-us/ai)

### Mocap / Asset Resources

- [Rokoko Studio](https://www.rokoko.com/products/studio)
- [Rokoko Create](https://create.rokoko.com/)
- [Rokoko mocap workflow](https://www.rokoko.com/insights/top-8-most-popular-motion-capture-and-animation-software)
- [Mixamo](https://www.mixamo.com/)
- [Mixamo rigging guide](https://helpx.adobe.com/creative-cloud/help/mixamo-rigging-animation.html)
- [Adobe Firefly](https://firefly.adobe.com/)

### AI / Tooling Resources

- [Unity MCP overview](https://docs.unity3d.com/Packages/com.unity.ai.assistant%402.0/manual/unity-mcp-overview.html)
- [Unity MCP get started](https://docs.unity3d.com/Packages/com.unity.ai.assistant%402.0/manual/unity-mcp-get-started.html)
- [Claude Code MCP](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Cursor MCP](https://cursor.com/docs/mcp)
- [Codex CLI docs](https://learn.chatgpt.com/docs/codex/cli)
- [MCP standard](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro)

## 14. Practical Recommendation

For this game, the best production path is:

1. Build player rigs and locomotion first.
2. Use Mixamo or marketplace clips only as support, not as the final cricket signature.
3. Use Rokoko or similar mocap for batting, bowling, and keeper-quality motion if budget allows.
4. Use Animation Rigging to make the motion fit the cricket context.
5. Use Cinemachine and Timeline to make scenes feel alive.
6. Use AI tools to accelerate setup, state wiring, and iteration.
7. Treat the final feel of batting and bowling as the highest animation priority.

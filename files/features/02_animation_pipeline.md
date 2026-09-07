# Feature 02: Animation Pipeline

## 1. What This Feature Is

This feature creates the cricket motion pipeline for every important character action in the game.

It is not just a clip collection. It is the system that makes the players feel like cricketers instead of generic humanoids.

It must support:

- batting
- bowling
- fielding
- wicketkeeper actions
- umpire signals
- celebration and reaction states
- replay and cinematic motion
- procedural correction and blending

## 2. Why This Feature Matters

In a cricket game, motion is part of the gameplay feel:

- batting timing looks wrong if the motion is vague,
- bowling feels weak if the run-up and release are not readable,
- fielding feels broken if handoffs and pickups are not clear,
- the game feels unfinished if replays and reactions are weak.

This feature defines the “cricket identity” of the game.

## 3. Rig and Retargeting Strategy

### 3.1 Shared Humanoid Rig

Build one standard humanoid rig that every player character can use.

The rig must support:

- bats,
- bowling actions,
- keeper crouches,
- fielding dives,
- throws,
- reaction poses.

### 3.2 Retargeting Rules

- All main motion should be retargetable across player models.
- Bone proportions should be consistent enough for clips to read well.
- The bat and throwing hand must align properly with gameplay events.

### 3.3 Animation Rigging Use

Use Animation Rigging for:

- bat alignment,
- hand placement,
- foot planting,
- bowling arm correction,
- look-at targeting,
- keeper reach correction,
- fielding aim correction.

This layer should fix motion to the game state instead of relying on perfect raw clips.

## 4. Clip Production Plan

### 4.1 Base Locomotion

Build the movement foundation first:

- idle,
- walk,
- jog,
- sprint,
- turns,
- starts,
- stops,
- pivots.

These clips are needed before any cricket-specific motion can feel stable.

### 4.2 Batting Motion

Create batting clips by shot type and timing quality.

You need:

- stance clips,
- pre-shot preparation,
- defensive motion,
- drive motion,
- cut motion,
- pull motion,
- sweep motion,
- lofted motion,
- improvised motion,
- mistimed outcomes,
- running between wickets.

Each major batting shot should support:

- early timing,
- good timing,
- late timing.

That timing matrix is critical because batting is one of the main skills of the game.

### 4.3 Bowling Motion

Bowling needs distinct forms for each major delivery class.

You need:

- pace run-up and release,
- yorker or short-length pace variation,
- seam delivery motion,
- off-spin release,
- leg-spin release,
- slower ball variation,
- follow-through and recovery,
- release quality variants.

Bowling should not look like one reused animation with a different ball outcome.

### 4.4 Fielding Motion

Fielding must cover:

- ready stance,
- sprinting,
- turning to the ball,
- clean pickup,
- sliding pickup,
- diving stop,
- short throw,
- long throw,
- relay throw,
- catch variants,
- missed catch,
- run-out reaction.

The player should be able to read what the fielder is trying to do.

### 4.5 Wicketkeeper Motion

Create keeper-specific motion for:

- crouch idle,
- standing up,
- taking edges,
- low takes,
- high takes,
- stumping,
- appeal,
- recovery.

The keeper is a special role and must not feel like generic fielding.

### 4.6 Presentation Motion

Create non-gameplay motion for:

- wicket celebration,
- boundary celebration,
- frustration,
- umpire signals,
- intro shots,
- result motion,
- replay closeups.

These motions make the game feel alive.

## 5. Unity Setup Tasks

### 5.1 Animator Controllers

- build controllers for batting, bowling, fielding, keeper, and reactions,
- use clear parameter names,
- keep locomotion separate from action layers,
- keep state transitions understandable for AI maintenance.

### 5.2 Blend Trees

Blend trees should be driven by gameplay variables such as:

- movement speed,
- shot type,
- timing quality,
- release quality,
- throw power,
- dive trigger.

### 5.3 Timeline

Use Timeline for:

- intro scenes,
- wicket replays,
- match presentation,
- menu motion,
- reward reveals.

### 5.4 Animation Events

Align gameplay events with animation events where needed:

- bat contact,
- ball release,
- catch frame,
- stump break,
- throw release,
- celebration trigger.

## 6. Expected Output

- Batting looks like batting.
- Bowling looks like bowling.
- Fielding looks responsive and readable.
- Keeper actions are special and clear.
- Replays and presentation motion exist.
- The game can begin tuning feel inside Unity.

## 7. Dependencies

- Feature 00 foundation
- Feature 01 data layer
- `animation_requirements.md`
- `animation_and_scene_pipeline_roadmap.md`
- `animation_clip_inventory.md`
- `scene_by_scene_setup.md`

## 8. References

- [animation_requirements.md](../animation_requirements.md)
- [animation_and_scene_pipeline_roadmap.md](../animation_and_scene_pipeline_roadmap.md)
- [animation_clip_inventory.md](../animation_clip_inventory.md)
- [scene_by_scene_setup.md](../scene_by_scene_setup.md)
- [unity_ai_workflow_and_project_structure.md](../unity_ai_workflow_and_project_structure.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- create Animator Controller scaffolds,
- create parameter enums or constants,
- create clip import helpers,
- create rigging setup scripts,
- create validation scripts for required clip coverage,
- create Timeline helper assets or scripts,
- create editor tools to inspect animation coverage.

## 10. Expected Tests

- retargeting tests,
- blend tree checks,
- timing variant checks,
- clip naming coverage checks,
- runtime motion validation in Unity,
- replay and presentation motion validation.

## 11. Exit Criteria

- The main cricket actions have animation coverage.
- The clips retarget correctly.
- The motion is usable inside Unity.
- Batting and bowling feel distinct.
- Presentation motion exists for key moments.

# Animation & Motion Requirements

## 1. Pipeline Decision (decide before sourcing any assets)

Options, roughly in order of fidelity vs cost:
1. **Professional mocap** (studio or service like Rokoko) — highest fidelity, highest cost/time, best if budget allows for at least the core batting/bowling actions.
2. **Marketplace/Mixamo-sourced + retargeted** — fast, cheap, generic; fine for fielding/running, likely too generic-looking for the signature batting/bowling actions that need to feel "cricket-specific."
3. **Procedural/IK-driven** — most flexible for blending (e.g., continuously variable bowling arm angle for different deliveries) but requires more animation-engineering time upfront.

**Locked decision:** hybrid. Use Rokoko or equivalent captured/bespoke motion for the 30 signature batting and bowling clips, Mixamo or licensed marketplace motion for approximately 40 locomotion/support clips, and Animation Rigging for procedural variation and correction. The source and license for every imported clip must be recorded before import; Mixamo is not permitted for signature batting or bowling clips.

### Production Budget

- One shared Unity Humanoid avatar at `1.8 m` reference height.
- Maximum player render mesh: `30,000` triangles for Mid/High, `18,000` for Low.
- Maximum runtime animation memory: `96 MB` on Low, `160 MB` on Mid, `240 MB` on High for player clips and controllers combined.
- Use 30 fps authored clips for gameplay actions unless a reference requires 60 fps; compress clips with Unity keyframe reduction after contact/release validation.
- Priority 1 clips must run on Low. Priority 2 clips may fall back to Priority 1 or a generic reaction. Priority 3 clips are optional on Low and may be Addressables content.

## 1.1 Locked Rig Standard

Use Unity Humanoid with a neutral T-pose, a `1.8 m` reference height, and stable Unity Human Bone mapping. Required bones are hips, spine/chest/upper chest, neck/head, clavicles, upper/lower arms, hands, upper/lower legs, feet, toes, and named bat-hand sockets. Bat and ball props attach through sockets, never through clip-specific world offsets.

## 2. Required Action Inventory

### Bowling
- Run-up (loop, variable length)
- Delivery actions by type: pace (standard, yorker-length variant), off-spin, leg-spin, variations (doosra/googly if in scope) — each needs distinct arm/body mechanics, not just a palette-swap of one animation
- Follow-through per delivery type

### Batting
- Stance/ready position (per footedness: front-foot lean, back-foot lean)
- Shot animations **by shot type × timing quality**: this is a matrix, not a flat list —
  - Shot types: defensive block, drive (straight/cover/on), cut, pull, hook, sweep, loft/six-hit
  - Timing quality per shot: early / good / late — each needs a distinct blend target so mistimed shots visibly and mechanically look mistimed (edges, mis-hits), not just "the good animation but weaker"
- Running between wickets (sprint, turn at crease, dive for crease)

### Fielding
- Idle/ready positioning per field position (close-in crouch vs outfield ready stance)
- Ground fielding: clean pickup, sliding stop/dive-stop
- Throws by distance/urgency: flat/direct throw, underarm flick (close-in), long-distance relay throw
- Catches: chest-height, low/diving, high/overhead, boundary-edge (with balance/out-of-play awareness if implementing that rule)
- Wicketkeeper-specific: crouch stance, stumping action, standing-up-to-spin variant

### Umpire / misc
- Basic umpire signal set (out, wide, no-ball, six, four, boundary) — low priority, can be simple/stylized

## 3. Blend Tree Parameters

Define these early since they drive both animation setup and gameplay input mapping:

- **Batting:** shot-direction (2D blend, stick input), timing-quality (early/good/late as a blend axis), power (hold-duration mapped to swing intensity)
- **Bowling:** delivery-type (discrete selection, not blended), release-timing-accuracy (affects a subtle "execution quality" blend — a slightly early/late release should visibly show effort/wobble even if mechanically resolved via physics, not animation, for the actual ball outcome)
- **Fielding:** movement-speed blend (idle/jog/sprint), dive-trigger (discrete), throw-power (blend axis)

The animation system consumes the same timing windows as physics: `60 ms` good and `120 ms` early/late boundary from `delivery_physics_constants.json`. Do not create separate animation timing constants.

## 3.1 Authoritative Event Timing

Gameplay clips must document normalized event times in a sidecar table:

| Event | Required normalized range | Authority |
|---|---:|---|
| `BatContact` | `0.36-0.44` | Server resolves outcome; client event is visual |
| `BallRelease` | `0.28-0.38` | Server spawns/validates delivery |
| `CatchPoint` | `0.45-0.65` | Server validates catch |
| `ThrowRelease` | `0.35-0.55` | Server validates throw |
| `StumpBreak` | `0.45-0.60` | Server resolves dismissal |

The exact frame is recorded per clip, but the gameplay event carries `deliveryId` and cannot be duplicated by a local animation callback.

## 4. Priority Order for Production

Given the milestone plan (physics-first), animation priority should follow:
1. Batting shots (needed for Nets mode milestone — this is the first playable)
2. Bowling actions (same milestone)
3. Basic fielding (pickup/throw) — needed for 2-player online milestone
4. Diving/catching variety, wicketkeeper set — needed before Main Mode milestone
5. Polish pass (follow-throughs, celebrations, umpire signals) — post-core-loop

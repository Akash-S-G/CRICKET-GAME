# Animation & Motion Requirements

## 1. Pipeline Decision (decide before sourcing any assets)

Options, roughly in order of fidelity vs cost:
1. **Professional mocap** (studio or service like Rokoko) — highest fidelity, highest cost/time, best if budget allows for at least the core batting/bowling actions.
2. **Marketplace/Mixamo-sourced + retargeted** — fast, cheap, generic; fine for fielding/running, likely too generic-looking for the signature batting/bowling actions that need to feel "cricket-specific."
3. **Procedural/IK-driven** — most flexible for blending (e.g., continuously variable bowling arm angle for different deliveries) but requires more animation-engineering time upfront.

**Recommendation:** hybrid — mocap or high-quality marketplace set for core batting shots and bowling actions (these define the game's feel), procedural IK layered on top for variation (arm angle, follow-through variance) and for fielding actions where full mocap coverage of every variation isn't practical.

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

## 4. Priority Order for Production

Given the milestone plan (physics-first), animation priority should follow:
1. Batting shots (needed for Nets mode milestone — this is the first playable)
2. Bowling actions (same milestone)
3. Basic fielding (pickup/throw) — needed for 2-player online milestone
4. Diving/catching variety, wicketkeeper set — needed before Main Mode milestone
5. Polish pass (follow-throughs, celebrations, umpire signals) — post-core-loop

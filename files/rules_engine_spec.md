# Rules / Umpire Engine Spec

## 1. Purpose

Cricket's rules have a lot of edge cases that are easy to get subtly wrong. This document is the state machine to implement against — write test cases directly from these states before considering the rules engine "done."

## 2. Match State Hierarchy

```
Match
 └─ Innings (1 or 2, per format)
     └─ Over
         └─ Delivery (ball)
```

- **Match state:** NotStarted → TossPhase → Innings1 → InningsBreak → Innings2 → Complete
- **Innings state:** InProgress → AllOut | OversComplete | TargetChased → Ended
- **Over state:** InProgress → Complete (6 legal deliveries, or mode-defined count) → BowlerChangeRequired
- **Delivery state:** Bowling → InFlight → BatterAction → BallDead → Resolved (run(s)/wicket/extra recorded)

Each transition should be an explicit event the rules engine emits (`OnOverComplete`, `OnWicketFallen`, `OnInningsEnd`, etc.) that other systems (UI, stats, AI) subscribe to — don't let those systems poll state.

## 3. Delivery Outcome Resolution

Each delivery resolves to exactly one primary outcome plus zero or more extras:

- **Primary:** dot ball / runs scored (1–6) / wicket
- **Extras (can stack with primary in real cricket):** wide, no-ball, bye, leg-bye
- **Wicket types to implement:** bowled, caught, LBW, run-out, stumped, hit-wicket (others like obstructing the field are edge cases — stub or defer)

## 4. LBW Logic

This is the most algorithmically involved rule. Standard decision tree:

1. Was the delivery legal (not a no-ball)? → if no-ball, LBW cannot be given regardless of below.
2. Did the ball pitch outside leg stump? → if yes, not out (regardless of impact).
3. Was impact (with pad) in line between wickets, OR was the batter offering no shot and impact was outside off but still in line enough per rule variant? → if impact clearly outside off stump AND batter attempted a shot, not out.
4. **Ball-tracking projection:** given the ball's trajectory and spin/seam at the moment of impact, project forward — would it have gone on to hit the stumps (within the stump width/height "zone")? This needs the same trajectory math as your swing/seam simulation, just projected past the impact point instead of stopping there.
5. If all above conditions favor the bowling side → OUT.

Build this as a pure function taking `(delivery_trajectory, pitch_point, impact_point, impact_type, shot_attempted: bool) → LBWResult` so it's independently testable against known real-world scenarios.

## 5. No-Ball Conditions (mode-dependent — check `ModeConfig.rule_overrides`)

- Front-foot fault (bowler's front foot lands full outside the popping crease)
- Height fault (full toss above waist height on-target = beamer; certain heights are automatic no-ball)
- Fielding restriction violations (too many fielders outside the ring during powerplay, illegal fielder positioning at time of delivery)
- **A no-ball typically grants a free hit on the next delivery** — track this as explicit delivery-level state, not just a score adjustment.

## 6. Run-Out / Stumping Timing Resolution

- This is a networking-sensitive rule: at the moment the bails are dislodged, whichever batter's ground position (crease line) is authoritative determines the outcome.
- Since this depends on both the ball/fielder action (throw arrives, wicket broken) and batter movement (running between wickets) which may be controlled by different clients, **the server's authoritative position snapshot at the exact tick the wicket is broken is the single source of truth** — do not let client-side prediction of either party's position influence the actual decision, only the visual presentation leading up to it.
- Consider implementing a brief "review window" visual (ball-tracking-style replay) even without full DRS, since these moments are naturally close and players will want to see why a decision was made.

## 7. DRS (stretch goal — separate sub-system if built)

- Review limit tracking per innings/team
- Ball-tracking visualization reusing the LBW projection math (§4) as the actual visual playback, not a separate mocked-up animation
- Ultra-edge/snickometer equivalent would require audio/contact-frame analysis — likely out of scope for v1; a simplified "did bat visibly contact ball" collision-frame check is a reasonable approximation

## 8. Testing Approach

Write this engine with unit tests against known real cricket scenarios (documented historical decisions or constructed edge cases) before integrating with physics/networking — it should be fully testable as pure logic against mocked trajectory data.

# Game Design Document (GDD)

## 1. Vision Statement

A near-real-physics cricket game built around full 22-player online matches (Main Mode), with additional casual modes (Gully Cricket, MinBoundary) sharing the same physics and animation core. Missing human players are seamlessly backfilled by AI, so a match never stalls waiting for a full lobby. One shared simulation, many modes.

## 1.1 Product Positioning

This project is aiming at the gap between two established cricket-game families:

- The Big Ant line prioritizes licensing breadth, modern presentation, career mode, and competitive online play.
- The older EA/Codemasters-era games often had broad mode variety but weaker visual fidelity, weaker AI, and more bugs.

The target here is to take the strongest parts of both:

- Simulation that holds up under serious play.
- Match flow that does not fall apart when a lobby is short a few players.
- Casual modes that share the same core physics instead of feeling like separate mini-games.
- A data-driven architecture that makes new modes, grounds, and player sets additive instead of code-heavy.

See `competitive_analysis.md` for the current market comparison and the feature gaps this design should close.


## 1.2 Mobile-First Product Requirements

This is a mobile-focused game first, so the design has to compete with current mobile cricket titles, not just console/PC cricket sims.

Required mobile-facing features:

- Touch-first controls with a very short learning curve.
- Fast match entry and short session modes that finish in a few minutes.
- Offline play for training, quick match, and career progression.
- Optional online multiplayer, but not as the only way the game stays interesting.
- A strong tutorial path for batting, bowling, and fielding on touch controls.
- Lightweight UI that remains readable on a phone screen.
- Device/performance tiers for low-end phones.
- Save/resume support for interrupted mobile sessions.
- Daily missions, challenges, or event loops to give the game reasons to return.
- Career or progression systems that work in short bursts.
- Commentary and match feedback that remains useful without a big-screen broadcast presentation.
- Configurable camera, control, and sensitivity options because mobile input comfort varies a lot between players.

The mobile version should feel complete on its own. PC/console-style depth is useful, but it cannot come at the expense of mobile usability or session length.


## 1.3 Mobile Feature Set

### Must-Have Features

These are required for the game to count as a real mobile-first cricket release:

- Touch-first batting, bowling, and fielding controls.
- First-run tutorial and practice flow.
- Quick Match and Nets as the primary entry points.
- Offline play for training and short matches.
- Save/resume for interrupted sessions.
- Lightweight UI that stays readable on phones.
- Device/performance tiers for low-end phones.
- Clear timing, line, and length feedback.
- Configurable camera, sensitivity, and input assists.
- Match sessions that comfortably fit 2-5 minute windows.
- Reliable fallback logic for partial lobbies or no-lobby play.
- Basic progression that works even when the player only has a few minutes.

### Should-Have Features

These features make the game competitive with current mobile cricket titles:

- Daily missions and reward loops.
- Short tournament ladders or seasonal events.
- Burst-friendly career progression.
- Mobile-friendly replay or highlight support.
- Stronger commentary and match feedback.
- Better onboarding for returning players after a long break.
- Basic customization for squads, kits, and player identity.
- Stronger fielding assist options for touch play.
- A more explicit retention loop that gives the player reasons to return daily.

### Later Features

These are useful, but they should not block the first mobile release:

- Deeper live-ops events.
- Advanced cosmetics.
- Social sharing and community content exchange.
- Expanded narrative career layers.
- More complex online ladders.
- Spectator/replay tools beyond simple highlights.
- Extra mode variants once the touch core is stable.


## 2. Modes

### Main Mode
- 11v11 online, real cricket rules, intended to be the flagship competitive mode.
- Formats:
  - T20: the default launch format because it is the cleanest balance of scope, session length, and spectator readability.
  - ODI: a longer-form mode for players who want pacing and tactical depth.
  - Test: stretch goal at first, because it requires the full day/session model, pitch wear, weather effects, and a stronger commentary/presentation layer.
- Full-size ground, full physics fidelity tier, full rules engine including LBW and run-outs.
- AI backfill enabled for any empty slot at match start or on disconnect.
- Matchmaking should prioritize complete human lobbies, but the game must remain playable with partial lobbies. That is a core differentiator, not a fallback.

### Gully Cricket
- 2-8 players, casual, fast matchmaking.
- Designed for short sessions, lower consequence gameplay, and rapid onboarding.
- Simplified rules:
  - No LBW by default, with optional toggle if a group wants stricter play.
  - Tip-and-run scoring option.
  - Informal field restrictions.
  - Faster over cadence and lighter penalties for mistimed actions.
- Small/street-style ground, simplified physics tier.
- Primary teaching ground for new players learning the control scheme.
- Should be the mode used for first-time tutorial flows, control familiarization, and mobile-friendly input experiments.

### MinBoundary
- 2v2-4v4, very small ground, boundaries close in.
- High-scoring, arcade-paced, short session length, target under 10 minutes per match.
- Simplified physics tier, mobile-first, but still using the same underlying ball and bat logic as Main Mode.
- This mode exists to test whether the core mechanics survive extreme tempo and tiny-space fielding pressure.
- Should be the easiest mode to pick up, but not a separate physics sandbox.

### Nets (build early, ship anytime)
- Single player, no opponent team.
- Bowling machine or AI bowler at adjustable difficulty and delivery type.
- Sandbox for players and developers to learn and tune the batting timing-window mechanic and bowling release mechanic in isolation.
- Nets mode is the primary tuning environment for:
  - batting timing windows,
  - shot response,
  - camera distances,
  - animation blend timing,
  - ball-tracking feel,
  - user onboarding.
- If Nets does not feel good, the rest of the game will inherit that weakness.

### Career (later)
- 1 human + AI teammates and AI opponents, full rules.
- Reuses the AI backfill system directly, so career mode does not require a separate AI architecture.
- Career is intentionally deferred until the simulation loop is stable because it depends on trustworthy stats, progression, and long-form match flow.
- Career mode should eventually absorb:
  - player progression,
  - club-to-international career arcs,
  - injury and form systems,
  - press or narrative events,
  - optional training between matches.

## 3. Control Scheme

### Batting

**Design basis:** researched across community/critic reception of Don Bradman Cricket (14/17), Cricket 24/26, and mobile titles (Boundary Bashers, Flick Cricket) before settling on this scheme. Key findings that shaped it:
- Don Bradman Cricket's dual-stick scheme (left stick = footwork, right stick = shot direction/type) is consistently cited as the most realistic-feeling cricket control system to date - reviewers described it as the first time a cricket game gave a genuine sense of control, precisely because footwork and shot direction are two independently meaningful inputs rather than one flattened "pick a shot" menu.
- Cricket 24 drew specific criticism for shot direction feeling disconnected from stick input - players reported aiming one way and the batter playing straight to the same fielder regardless. Cricket 26 addressed this and was praised for finally making stick input reliably translate to actual shot direction. **Takeaway: whatever input scheme we build, direction input must deterministically map to outcome (modulated by timing/mistiming), never feel "predetermined."**
- A recurring complaint (PlanetCricket community) is that some games force early pre-commitment to shot type (e.g. pressing a defense button well before contact), which breaks realism - real batters can adjust intent until very late. **Takeaway: shot-type modifiers should resolve as late as possible, ideally at the same moment as timing input, not as an earlier separate step.**
- Mobile titles succeed with simplified swipe/tap schemes rather than full dual-stick, since touch input can't replicate two independent analog sticks comfortably.

Design goals for batting:

- Make the shot decision readable without turning it into a menu.
- Preserve late intent changes so the player can still adjust under pressure.
- Make the same input scheme support defensive batting, strike rotation, and attacking shots.
- Ensure timing failure produces believable cricket outcomes: edges, top-edges, mishits, misreads, and wickets.
- Make left/right movement matter enough that bowlers can exploit weak side play.

**PC/Console (Main Mode) - dual-stick, Don Bradman-style, hardened against the Cricket 24 direction-fidelity problem:**
- Left stick: footwork / crease positioning - front-foot or back-foot commitment, advance/retreat down the pitch
- Right stick: shot direction, flicked toward the intended target area (360 degrees around the batter)
- Trigger/bumper modifiers (held, not pre-pressed): shot character - defensive, aggressive/power, or unorthodox (reverse sweep, scoop) - layered on top of the stick input at the moment of the shot, not committed earlier, so intent can change until the last instant
- Timing: a single press/release at the moment of bat-ball contact, independent of direction/type inputs, feeding the early/good/late quality window
- Timing quality (early / good / late) directly affects contact quality - not just cosmetic, it changes exit velocity and directional accuracy (see TDD physics section) - and direction input must reliably produce the aimed-for shot when timing is good, with believable degradation (mishits, edges) only as timing quality drops, never as an unexplained direction override

**Mobile (Gully / MinBoundary) - simplified swipe scheme:**
- Swipe direction = shot direction (reduced angle granularity vs. full stick, tuned for touch)
- Swipe speed/duration = power
- Timing = release point of the swipe relative to ball arrival
- No separate footwork input on mobile - footwork is context-automated (batter auto-adjusts front/back foot based on delivery length) to keep touch controls learnable in seconds, matching what works in successful mobile cricket titles

#### Batting Input-to-Animation Contract

All platforms resolve the same gameplay intent fields before selecting an animation:

```text
shotType: enum { Defense, StraightDrive, CoverDrive, OnDrive, Cut, Pull, Hook, Sweep, Loft }
shotDirectionDeg: float [-180, 180]
timingQuality: float [-1, 1]       // -1 early, 0 good, +1 late
shotPower: float [0, 1]
footworkContext: enum { FrontFoot, BackFoot, Neutral }
```

- `timingQuality` is calculated against `delivery_physics_constants.json`: good is within `60 ms`, early/late remains usable within `120 ms`.
- `shotPower` is the normalized hold/swipe input after clamping; it drives the animation power axis and the physics input, not a separate visual-only effect.
- PC/console footwork comes from the left stick. Mobile footwork is selected from delivery length: front foot for full/overpitched, back foot for short, neutral for good length until the shot commits.
- `shotType`, `timingQuality`, `shotPower`, and `footworkContext` must be recorded in the delivery intent so animation, physics, replay, and networking consume the same decision.
- The animation mapping is defined in `animation_and_scene_pipeline_roadmap.md`; no feature may invent a second timing enum.

### Bowling
- Run-up: automatic or player-paced approach
- Delivery type select: pace variation, seam angle, spin type/variation - chosen before or during run-up depending on mode's complexity tier
- Release timing: button press at run-up's release point determines line/length accuracy; mistimed release = execution error (ball drifts from intended target)
- Bowler identity must matter:
  - pace bowlers should feel distinct from spinners,
  - swing and seam should create meaningful line movement,
  - variations should be readable but not overexposed,
  - fatigue should affect control over longer spells.
- Bowling feedback should teach the player what went wrong rather than simply stating "missed."

### Fielding
- **Active fielder** (ball within the mode's `handoff_radius_m`): direct movement + dive/slide + throw aim + throw power, human-controlled
- **Inactive fielders:** AI-controlled positioning until the server predicts an intercept inside the configured handoff radius (full mechanics of this in `controller_handoff_spec.md`)
- Main Mode starts at `3.5 m`, Gully at `5.0 m`, and MinBoundary at `4.0 m`; values come from `mode_config_schema.json`.
- **Pre-ball field setting:** human captain/player sets fielder positions via drag-and-drop UI before each over or each ball (mode-dependent)
- Fielding should support:
  - close-in pressure catching,
  - boundary running and relay throws,
  - stumping and run-out pressure,
  - AI coverage that does not look like players are teleporting into optimal positions.

## 4. Match Flow

1. Toss (human or AI captain choice: bat/bowl, or heads/tails call in online lobby)
2. Innings start - batting order set (human players occupy specific batting slots; AI fills rest per mode's `min_players_to_start`)
3. Per over: field-setting phase - 6 (or mode-defined) deliveries - over change (ends swap, bowler change)
4. Wicket handling: dismissal type determined by rules engine - next batter walk-in - strike/non-strike reassignment
5. Innings end: all-out, overs complete, or target chased (2nd innings) - result screen
6. Post-match: stats summary, XP/progression awarded, replay/highlights (stretch goal)

Presentation requirements:

- Every major state change should be visible and understandable without reading a debug overlay.
- Result screens should explain the path to victory or defeat.
- Replays/highlights should be added after the core loop, but the data model should already preserve the information needed to generate them later.

## 5. Progression Systems (initial scope)

- Player XP per match (runs, wickets, catches, match result)
- Cosmetic unlocks (kit colors, bat/ball skins) - no gameplay-affecting unlocks in Main Mode, to keep competitive integrity
- Casual-mode-only unlocks can be more liberal (Gully/MinBoundary are less about competitive balance)
- Progression should not distort balance in Main Mode.
- Any unlock that changes performance should remain outside ranked or competitive play.

## 6. Fielding / Control-Handoff Summary

(Full spec lives in `controller_handoff_spec.md` - this section is the design-level summary.)

- Every fielding position starts under AI control except when actively occupied by a human at match/over start.
- The moment the ball's predicted trajectory intersects a radius around a human-controlled-eligible fielder, control transfers to that human (if online and not already occupied with another action).
- If two eligible humans qualify simultaneously, priority goes to whichever is closer to the ball's predicted landing/interception point.
- On disconnect mid-action, AI takes over from the exact physical state (position, momentum, animation) the human left - no snapping or freezing.

This system is a major differentiator relative to most cricket games, which either:

- keep fielding fully manual and therefore brittle online, or
- keep fielding mostly AI-driven and therefore less expressive.

The goal here is to allow real control where it matters, while making the game still function when the lobby is imperfect.

## 7. Non-Goals (explicitly out of scope for v1)

- Real player names/likenesses (licensing - separate legal track, see `licensing_notes.md`)
- Full broadcast-style camera direction / commentary AI (stretch goal, not core)
- Weather/day-night dynamic simulation beyond a static per-match setting
- Cross-play matchmaking nuance beyond "PC/console together, mobile separate pool" (revisit post-launch)
- Official stadium licensing breadth beyond whatever is legally available at launch
- Career mode polish before the core match loop is stable
- Sophisticated spectator tools or esports broadcast tooling before the game itself is compelling to play


## 8. Camera Modes

The game should support multiple cameras so different players can choose immersion, readability, or tactical awareness.

### Camera Types

- FPP / Batter View: close, immersive batting view for Nets and skill play.
- Batting Broadcast Camera: default batting camera for most modes.
- Bowling End Camera: delivery-focused view for bowling control.
- Mid-Wicket Tactical Camera: wide strategic view for field planning.
- Chase Camera: dynamic camera for running, fielding, and boundary pressure.
- Top-Down Tactical Camera: overview camera for casual and mobile-friendly modes.
- Replay Camera: cinematic highlight and wicket camera.

### Camera Defaults

- Nets: FPP or Batting Broadcast.
- Main Mode batting: Batting Broadcast.
- Main Mode bowling: Bowling End.
- Gully: Top-Down Tactical or Batting Broadcast.
- MinBoundary: Chase Camera or Top-Down Tactical.
- Career: Broadcast with highlight replays.

### Camera Rules

- Camera switching must be quick and never block play.
- Mobile players should be able to lock a preferred camera.
- The game should suggest a default camera based on mode and skill level.
- Camera presentation should help the player understand timing, field placement, and shot direction.

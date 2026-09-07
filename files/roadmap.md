# Milestone / Vertical Slice Roadmap

Order is deliberate: prove the riskiest, most foundational assumptions first. Do not start a milestone until the previous one's exit criteria are met — this project fails if physics/networking risk is discovered late.

## Milestone 0: Documentation & Data Schemas (current stage)
- All docs in `/docs` drafted (this set)
- All schemas in `/data` defined with placeholder values
- **Exit criteria:** schemas can represent a full mock match config without needing structural changes

## Milestone 1: Physics Prototype
- Ball flight, swing/seam aerodynamics, bat-ball collision, ball-pitch bounce interaction
- Single player, no networking, no real art (primitive shapes acceptable)
- **Exit criteria:** ball behavior feels physically plausible in isolated testing — swing is visible and controllable via delivery parameters, mistimed vs well-timed bat contact produces clearly different outcomes

## Milestone 2: Nets Mode (first playable)
- Single player, real bowling machine/AI bowler, human batting with full timing-window mechanic
- Core batting animations integrated (per `animation_requirements.md` priority order)
- **Exit criteria:** a new player can pick up the controls and hit varied shot types within a few minutes; this is your primary internal playtesting tool going forward

## Milestone 3: Controller Abstraction (solo)
- Implement `IPlayerController`, `HumanController`, `AIController` per `TDD.md` §4
- AI can bat, bowl, and field solo (no network yet) — validates the abstraction independent of netcode complexity
- **Exit criteria:** a full solo match (human vs AI, human fielding vs AI batting) can be played start to finish

## Milestone 4: Local/LAN 2-Player
- Smallest possible multiplayer proof — validates handoff logic and NGO setup without full-scale netcode complexity
- Explicitly test control-handoff smoothness with artificial latency injection (100–150ms) per `controller_handoff_spec.md` §6
- **Exit criteria:** handoff between AI and human fielders is visually seamless under realistic latency; disconnect/reconnect handled per spec

## Milestone 5: Full Networked Small Match (Gully, 2v2 Online)
- Real internet conditions (not LAN), dedicated/host server model finalized
- **Exit criteria:** a full Gully match completes online with acceptable perceived latency and no desync bugs in run-outs/catches

## Milestone 6: Main Mode, 22-Player Online
- Full interest-management/bandwidth optimization work (per `TDD.md` §3) — this is the milestone most likely to reveal the need to escalate from NGO to Photon Fusion; make that call here, not earlier
- Full physics fidelity tier, full rules engine (including LBW)
- **Exit criteria:** a full 11v11 match completes with acceptable performance/bandwidth and correct rules resolution across a full innings

## Milestone 7: MinBoundary Mode
- Should be near-free at this point if the mode-config data-driven system was built correctly — primarily a new `ModeConfig` asset + a small ground asset + mobile performance pass
- **Exit criteria:** validates the "new mode = data file, not new code" architecture goal

## Post-core-loop (ongoing)
- Career mode (reuses AI backfill system)
- Test-match-length format, pitch wear simulation
- Progression/cosmetics, matchmaking polish, replay/highlights


## Mobile Priority Ladder

See `mobile_roadmap.md` for the mobile-first delivery order.

### Must-Have

- Touch-first controls for batting, bowling, and fielding.
- First-run tutorial and practice drills.
- Quick Match and Nets mode.
- Offline play.
- Save/resume.
- Mobile-safe UI and performance tiers.
- Clear timing and delivery feedback.

### Should-Have

- Daily missions and reward loops.
- Short tournaments or event ladders.
- Career/progression that works in short bursts.
- Better commentary and mobile-friendly highlights.
- Stronger assist options and post-break return onboarding.

### Later

- Deeper live ops.
- Advanced cosmetics.
- Social sharing.
- Expanded narrative career.
- More complex online ladders.

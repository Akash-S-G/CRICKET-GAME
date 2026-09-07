# Production System Design

## 1. Goal

Design a mobile-first cricket game that is:

- fast to start,
- playable offline,
- ready for online multiplayer later,
- scalable for live updates and progression,
- maintainable by AI coding tools and humans,
- production-ready rather than prototype-only.

The key system choice is to keep the game architecture modular, data-driven, and server-authoritative where competition matters.

## 2. Design Principles

1. Separate simulation from presentation.
2. Keep the same game rules whether the match is offline or online.
3. Make mobile the first-class runtime target.
4. Use data assets for content, not hard-coded mode logic.
5. Make the app resilient to short sessions, interrupted play, and partial lobbies.
6. Treat multiplayer as an extension of the same game session model, not a separate product.
7. Use production services for identity, save, config, and hosting instead of hand-rolled systems.

## 3. High-Level Architecture

```text
                    +------------------------------+
                    |        Client App            |
                    |------------------------------|
                    | UI / Camera / Audio / Haptics|
                    | Presentation State          |
                    | Match Session Controller     |
                    | Simulation Adapter           |
                    | Local Save / Cache           |
                    +--------------+---------------+
                                   |
                                   | local events / network messages
                                   v
                    +------------------------------+
                    |        Game Domain           |
                    |------------------------------|
                    | Rules Engine                 |
                    | Match State Machine          |
                    | Ball / Bat / Fielding Sim    |
                    | Progression / Rewards        |
                    | Mode Config / Player Data    |
                    +--------------+---------------+
                                   |
                         offline or online session
                                   |
               +-------------------+-------------------+
               |                                       |
               v                                       v
    +-----------------------+              +--------------------------+
    | Offline Session Path  |              | Online Multiplayer Path  |
    |-----------------------|              |--------------------------|
    | Local authoritative   |              | Dedicated server         |
    | simulation            |              | authoritative simulation  |
    | local save/progression |              | matchmaking / lobby       |
    +-----------------------+              | replication / reconcil.   |
                                           +--------------------------+
```

## 4. Client Architecture

The client should be a thin presentation shell around a shared game domain.

### 4.1 Runtime Layers

- Presentation layer
  - UI
  - camera
  - animation
  - audio
  - haptics
  - VFX

- Session layer
  - app boot
  - login
  - profile selection
  - mode selection
  - match setup
  - match lifecycle
  - post-match rewards

- Domain layer
  - cricket rules
  - match state machine
  - ball physics decisions
  - batting/bowling/fielding actions
  - progression rules

- Data layer
  - ScriptableObjects for authored content
  - JSON schemas for source-of-truth balance data
  - cached player/profile/session state

### 4.2 Why This Split

This structure keeps the game testable:

- simulation logic can be unit tested,
- UI can be swapped without changing the rules engine,
- mobile performance can be tuned without rewriting game rules,
- multiplayer can reuse the same domain layer as offline play.

## 5. Core Game Session Model

Every play session should be modeled the same way:

1. App boot
2. Authentication / profile load
3. Content config load
4. Home screen
5. Match setup
6. Session start
7. Real-time play
8. Result and rewards
9. Save / sync / analytics flush

The same session object should back:

- Quick Match,
- Nets,
- Gully,
- MinBoundary,
- Career,
- Online Match.

Only the session source changes:

- local simulation for offline,
- authoritative server for online.

## 6. Game Domain Design

### 6.1 State Machines

The game should be driven by explicit state machines:

- App state: boot, auth, home, loading, in-match, results, offline fallback
- Match state: toss, innings, over, delivery, dead ball, result
- Player state: idle, batting, bowling, fielding, transitioning, disconnected
- Camera state: default, override, replay, cutscene, training
- Progression state: unlock pending, reward claim, synced, stale cache

### 6.2 Domain Rules

The domain layer should own:

- wicket logic,
- scoring logic,
- extras,
- fielding handoff,
- batting timing windows,
- bowling execution quality,
- AI fill behavior,
- match completion conditions.

UI and camera should only react to domain events.

### 6.3 Event Model

Use domain events for important transitions:

- `OnMatchStarted`
- `OnTossComplete`
- `OnBallDelivered`
- `OnShotPlayed`
- `OnWicketFallen`
- `OnOverComplete`
- `OnInningsEnded`
- `OnRewardGranted`

This avoids polling and keeps presentation decoupled from rules.

## 7. Mobile-First Runtime Architecture

### 7.1 Device Tiers

The app should support at least three performance tiers:

- Low: minimum-viable visuals, aggressive culling, simplified crowds and effects
- Mid: default mobile quality
- High: better shadows, crowd density, camera polish, replay fidelity

### 7.2 Mobile Optimization Strategy

Use official Unity mobile optimization guidance as the baseline:

- minimize materials and state changes,
- use texture atlases,
- keep shader complexity low,
- reduce alpha-test-heavy rendering,
- watch fill rate and overdraw,
- optimize distribution size for mobile installs.

### 7.3 Mobile Session Needs

The mobile architecture must support:

- fast launch,
- short match sessions,
- pause/resume or save/resume,
- low memory churn,
- offline-first play,
- touch-driven camera selection,
- one-handed UI paths where practical.

## 8. Content System

### 8.1 Data-Driven Assets

Use data assets for:

- mode configs,
- player profiles,
- pitch types,
- ground types,
- camera configs,
- progression tuning,
- difficulty tuning,
- cosmetic catalogs.

### 8.2 Addressable Content

Use Addressables for content that should be:

- loaded on demand,
- updated without full app rebuilds,
- packaged for seasonal or live-ops delivery,
- separated from the base install.

This is especially important for:

- stadiums,
- cosmetic packs,
- tutorial overlays,
- replay assets,
- optional training content.

### 8.3 Content Delivery

Use remote content distribution for non-core assets so the base mobile install stays small and updates stay manageable.

## 9. Persistence and Identity

### 9.1 Local Data

Store locally:

- last selected mode,
- camera preference,
- control sensitivity,
- tutorial completion,
- cached offline progression,
- temporary session state.

### 9.2 Cloud Data

Store in cloud services:

- player identity,
- progression,
- cosmetics,
- long-term profile state,
- cloud-synced settings,
- multiplayer entitlement or session metadata.

### 9.3 Save Strategy

Use a split save model:

- local cache for instant startup and offline resilience,
- cloud sync for cross-device continuity,
- conflict resolution based on the most recent trusted server state.

## 10. Multiplayer Architecture

### 10.1 Current Direction

Use a server-authoritative model for anything competitive or physics-sensitive.

For Unity, the production path should be:

- NGO for the early network path and smaller-scale matches,
- dedicated server model for authoritative competition,
- reevaluate Photon Fusion only if NGO cannot carry the full 22-player target.

### 10.2 Multiplayer Services

Use managed multiplayer services for:

- authentication,
- matchmaking,
- session allocation,
- server hosting,
- cloud save,
- remote config,
- live tuning.

### 10.3 Session Model

The multiplayer session should be represented as:

- one authoritative match instance,
- multiple clients,
- explicit ownership for batting and fielding control,
- server-owned rules and ball resolution,
- client-side prediction only where it improves responsiveness.

### 10.4 Why Dedicated Server Matters

Dedicated server is the right long-term model because:

- cricket is sensitive to authoritative ball state,
- fielding handoff needs consistent ownership,
- run-outs and stumping need trustworthy timing,
- the game will eventually need fair online competition.

## 11. Networking Rules

1. The server decides the match outcome.
2. Clients may predict their own immediate inputs.
3. The ball is always authoritative.
4. Rules resolution never depends on a client guess.
5. Handoffs are explicit events, not inferred state.
6. Latency should be masked visually, not hidden in logic.

## 12. LiveOps and Tuning

### 12.1 Remote Config

Use remote config for:

- difficulty tuning,
- tutorial values,
- economy tuning,
- camera defaults,
- event activation,
- performance tier defaults,
- feature rollout flags.

### 12.2 Cloud Code / Server Logic

Use server-side code for:

- reward validation,
- challenge progression,
- anti-tamper checks,
- daily missions,
- timed events,
- session validation,
- progression reconciliation.

### 12.3 Analytics

Instrument the game for:

- tutorial completion,
- match start/dropoff,
- shot success rates,
- bowling accuracy,
- fielding handoff failures,
- session length,
- retention funnels,
- crash frequency,
- device performance by tier.

## 13. Security and Integrity

The game should assume that client data can be modified.

Therefore:

- reward logic should be validated server-side,
- competitive results should be server-owned,
- progression changes should be audited,
- sensitive tuning values should be remote-configured, not hard-coded,
- matchmaking should be authenticated.

## 14. Production Build Pipeline

### 14.1 Build Steps

- code review,
- automated tests,
- build verification,
- mobile package generation,
- server build generation,
- smoke testing,
- staged rollout.

### 14.2 Environments

Maintain at least:

- local/dev,
- QA/staging,
- production.

### 14.3 CI/CD

Use CI to validate:

- compile integrity,
- unit tests,
- basic play mode tests,
- mobile build success,
- dedicated server build success,
- content schema validity.

## 15. Observability and Recovery

Production-ready means the game must fail safely.

Track:

- crashes,
- ANRs or freezes on mobile,
- disconnect rates,
- matchmaking failures,
- server tick spikes,
- rules desyncs,
- content load failures.

Recovery behavior:

- fallback to offline mode if services fail,
- fallback to cached content if remote delivery fails,
- preserve last known safe profile state,
- retry sync on next launch.

## 16. Recommended Tech Choices

- Engine: Unity 6 LTS
- Rendering: URP
- Runtime UI: Unity UI / retained-mode game UI for production readability
- Editor tooling: UI Toolkit where useful
- Networking: NGO first, dedicated server path for competition, Photon Fusion only if needed later
- Content: Addressables + remote delivery
- Persistence: local cache + cloud save
- Config: Remote Config
- Multiplayer services: authentication, matchmaking, hosting, server allocation

## 17. Implementation Order

1. Build the domain model and match state machines.
2. Build the single-player simulation and Nets mode.
3. Add data-driven configs and Addressables.
4. Build mobile UI and onboarding.
5. Add local save, profile, and cloud sync.
6. Add small-scale multiplayer.
7. Add dedicated server support and scale to Main Mode.
8. Add live-ops, events, analytics, and retention systems.

## 18. Final Rule

If a feature cannot survive:

- mobile constraints,
- offline play,
- server authority,
- content updates,
- and AI-assisted maintenance,

then it is not production-ready for this game.

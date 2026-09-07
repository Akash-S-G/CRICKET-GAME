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

Concrete implementation specs live in [system_design/INDEX.md](system_design/INDEX.md). Use those subdocs for exact service contracts, storage rules, input maps, addressables, and build tooling.

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

### 6.1 State Machines (locked per `system_design/state_machines_and_event_model.md:8`)

The game is driven by typed state machines (no string polling):

- App: `Boot->Auth->Home->Loading->InMatch->Results`, plus `OfflineFallback`/`ErrorRecovery` from any state on failure `system_design/state_machines_and_event_model.md:16`
- Match: `NotStarted->TossPhase->Innings1->InningsBreak->Innings2->Complete` `system_design/state_machines_and_event_model.md:28`
- Delivery: `Bowling->InFlight->BatterAction->BallDead->Resolved` `system_design/state_machines_and_event_model.md:34`
- Player: `Idle/Batting/Bowling/Fielding/Transitioning/Disconnected` maps to `TDD.md:50` `ControllerState` + 0.15s blend `TDD.md:75`
- Camera: `Default/Override/Replay/Cutscene/Training` `system_design/state_machines_and_event_model.md:40`
- Progression: unlock pending, reward claim, synced, stale cache

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

### 6.3 Event Model (typed bus, tick-ordered)

Events are strongly typed payloads `system_design/state_machines_and_event_model.md:43`, domain emits before presentation:

- `OnBootStarted`/`OnAuthResolved`/`OnContentReady` `system_design/state_machines_and_event_model.md:33`
- `OnMatchStarted`/`OnTossComplete`/`OnBallDelivered`/`OnShotPlayed`/`OnWicketFallen`/`OnOverComplete`/`OnInningsEnded`/`OnRewardGranted` `system_design/state_machines_and_event_model.md:33`
- `OnHandoffTriggered`/`OnHandoffConfirmed`/`OnSaveConflict` `system_design/state_machines_and_event_model.md:33`

Bus: domain `C# event` or typed bus, no string broadcasts. Payload carries `DeliveryId` + `ServerTick` `TDD.md:58` `PhysicalState`. Replication uses intent `shotType/timingQuality/shotPower/deliveryType` `TDD.md:41` not Animator transforms `system_design/physics_tick_and_reconciliation.md:24`.

## 7. Mobile-First Runtime Architecture

### 7.1 Device Tiers (locked: Recommended Snapdragon 660 / 4GB baseline)

The app must support three locked tiers. Budgets are enforced per tier and drive URP asset choice.

| Tier | Target Device | FPS | Tris/Frame | Draw Calls | Shadows | Crowd | Texture Budget | Install |
|---|---|---|---|---|---|---|---|---|
| **Low** | Snapdragon 660, Adreno 512, 3-4GB RAM, Android 8+ / iPhone 8 | 30 | <1.5M | <70 | Off | 25% sprites, no 3D crowd | 1K atlases, ETC2 | <120 MB base |
| **Mid** | Snapdragon 720G, Adreno 618, 4-6GB | 45-60 | <2.5M | <100 | 512 low | 50% | 2K atlases | <150 MB |
| **High** | Snapdragon 865+, Adreno 650+, 6GB+ | 60 | <3.5M | <130 | 1024 med | 100% 3D | 2K+ | <200 MB |

Rules:
- Low uses Priority-1 animation only (`animation_clip_inventory.md:345`), no keeper layer, single IK pass.
- Selection auto on boot via `SystemInfo.processorCount` + `SystemInfo.systemMemorySize` + `Application.persistentDataPath` benchmark, override in Settings.
- URP assets: `URP-Low` (no shadows, no HDR), `URP-Mid` (hard shadows), `URP-High` (soft shadows + post bloom limited) per `unity_ai_workflow_and_project_structure.md:18`.

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

### 8.2 Addressable Content (locked: Unity CCD)

Groups and delivery per `system_design/addressables_grouping.md:1`:

| Group | Included in Base | Delivery | Example Assets |
|---|---|---|---|
| `Boot` | Yes <15 MB | Local | splash, core fonts, UIRoot |
| `CoreGameplay` | Yes | Local | ball/bat/pitch, core HUD, `ModeConfig` SO |
| `Tutorial` | No | On-demand at `Home` | overlays, VO |
| `Nets` | No | On-demand | nets props |
| `Stadiums` | No | Remote CCD | stadium mesh 10-20 MB each, crowd |
| `Cosmetics` | No | Remote CCD | kits, badges |

Rule: Match-critical assets never gate `Match_Play` load. Remote failure falls back to `ground_schema.json:4` `capacity` default + flat pitch.

### 8.3 Content Delivery

Unity CCD hosts remote catalog; local bundle copy for `Stadiums` fallback. Bounded LRU cache, never evict `Boot`/`CoreGameplay`. Use `IContentService` `system_design/service_interfaces.md:42` `PreloadBootContentAsync` / `LoadModeContentAsync`.

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

### 9.3 Save Strategy (locked: file + Unity Cloud Save)

Layout per `system_design/save_schema_versioning.md:16`: `Application.persistentDataPath/profiles/<profileId>/profile.json` + `.backup.json` + `.meta.json`, JSON UTF-8, `PlayerPrefs` never for profile. Services: `ISaveService` + `IAuthService` + `ISessionService` `system_design/service_interfaces.md:16`.

- Local cache for instant boot; Cloud Save (UGS) for continuity via `ISaveService` `LoadProfile`/`SaveProfile` with atomic temp-file swap.
- Every object carries `schemaVersion`/`dataVersion`/`updatedAtUtc`/`sourceAppVersion` `system_design/save_schema_versioning.md:33`. Migrations `ProfileDataV2->V3` pure, tested.
- Conflict: field-level merge, server wins for progression/unlocks, local wins for transient settings if newer `system_design/save_schema_versioning.md:46`. Record `save_conflict_detected` analytics `system_design/remote_config_and_analytics.md:34`.

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

## 14. Production Build Pipeline (locked: GitHub Actions + GameCI)

Spec `system_design/build_pipeline.md:1`:

### 14.1 Build Steps

- code review (main protected, PR required),
- `Unity -runTests -testMode EditMode` + `PlayMode` smoke,
- build verification (GameCI `builder@v4`),
- mobile APK/AAB + server headless (`dedicatedServer` target) generation,
- smoke: `Boot->Home->Nets` auto test `features/08_testing_ci_and_release.md:16`,
- staged: internal -> QA -> prod.

### 14.2 Environments

- local/dev (local `Application.persistentDataPath`),
- QA/staging (UGS staging, CCD staging catalog),
- production (UGS prod).

### 14.3 CI/CD

On every `feat/*` push: restore packages, `EditMode` + `PlayMode` tests, build Android, validate `mode_config_schema.json:2` via `ajv`, publish logs + `build_hash`. Git LFS tracks `*.png *.fbx *.wav *.unity` `system_design/build_pipeline.md:34`. LFS resolve verified in CI. Fail fast on compile.

## 15. Observability and Recovery (UGS + Crash Reporting, offline-buffered)

Track per `system_design/observability_stack.md:16` + `system_design/remote_config_and_analytics.md:34` events `session_start/match_start/delivery_resolved/handoff_triggered/save_conflict_detected`:
- crashes/ANRs (Crash Reporting via UGS Diagnostics + breadcrumbs `system_design/observability_stack.md:24`),
- disconnect rates, matchmaking failures, server tick spikes `physics_tick_and_reconciliation.md:16` 30Hz,
- rules desyncs, content load failures.

Recovery (fail-safe `system_design/security_threat_model.md:33`):
- offline fallback if `IAuthService.RestoreSessionAsync` `system_design/service_interfaces.md:26` fails,
- cached `Stadiums` if CCD fetch fails `system_design/addressables_grouping.md:31`,
- preserve last good `profile.backup.json` `system_design/save_schema_versioning.md:16`,
- retry cloud sync on next `OnBootStarted` `system_design/state_machines_and_event_model.md:33`,
- never trust client for `OnRewardGranted` `system_design/security_threat_model.md:33` server validates.

## 16. Recommended Tech Choices (pinned before Feature 00)

| Choice | Locked Value | Where Pinned |
|---|---|---|
| Engine | Unity 6 LTS `6000.0.x` exact patch in `ProjectSettings/ProjectVersion.txt` | `TDD.md:5` |
| Rendering | URP 17.x (3 assets Low/Mid/High `system_design.md:188`) | `TDD.md:6` |
| Runtime UI | uGUI; UI Toolkit editor-only | `TDD.md:7` + `system_design/ui_architecture_and_navigation.md:8` uGUI |
| Animation | Animation Rigging 1.2.x, Cinemachine 3.0.x, Timeline | `TDD.md:8` |
| Input | Input System 1.11.x, Action Maps `system_design/input_action_maps.md:6` | `system_design.md:32` |
| Networking | NGO 2.4.x server-authoritative, Fusion 2 fallback only at Main Mode go/no-go `TDD.md:9` | `TECH_STACK.md:15` |
| Content | Addressables 2.x + CCD `system_design/addressables_grouping.md:1` | `system_design.md:240` |
| Persistence | `ISaveService` file JSON `system_design/save_schema_versioning.md:16` + UGS Cloud Save | `system_design.md:275` |
| Config/Analytics | UGS Remote Config + Analytics `system_design/remote_config_and_analytics.md:8` | `system_design.md:333` |
| Services | UGS Auth/Cloud Save/Remote Config/Analytics/Relay/Lobby | `system_design.md:295` |
| Project Structure | `Assets/_Project/...` per `unity_ai_workflow_and_project_structure.md:18` (authoritative) | `TDD.md:105` |

Main Mode PC/console uses same project, `Gully`/`MinBoundary` simplified tier `TDD.md:81` `TECH_STACK.md:28`.

## 17. Implementation Order

Follow [implementation_master_plan.md](implementation_master_plan.md) for phase order. Do not use this section to override the execution plan.

The architecture rule is:

1. Lock the domain and service contracts first.
2. Build the single-player simulation and Nets mode next.
3. Add data-driven configs, Addressables, and persistence.
4. Build mobile UI and onboarding.
5. Add small-scale multiplayer.
6. Scale to dedicated-server support and live operations.

## 18. Final Rule

If a feature cannot survive:

- mobile constraints,
- offline play,
- server authority,
- content updates,
- and AI-assisted maintenance,

then it is not production-ready for this game.

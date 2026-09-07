# Scene by Scene Unity Setup

This document defines the exact scene structure for the game and what each scene must contain. The goal is a production-ready Unity layout that is easy for AI tools and humans to maintain.

## 1. Scene Strategy

- Keep scenes focused and small.
- Use additive loading for heavy gameplay content.
- Avoid one giant scene that contains the entire app.
- Treat UI, gameplay, tutorial, and replay as separate responsibilities.
- Keep mobile startup fast by using a lightweight bootstrap scene.

## 2. Scene List

### 2.1 `Boot`

Purpose:

- initialize services,
- set quality settings,
- load remote config,
- initialize auth,
- prepare save data,
- route into the correct next scene.

Must contain:

- splash presentation,
- version check,
- service bootstrap,
- fallback/error state,
- loading progress UI.

### 2.2 `Login_Profile`

Purpose:

- login or local profile selection,
- account binding,
- first-time player profile creation.

Must contain:

- profile list,
- create profile flow,
- sign-in state,
- guest/offline fallback,
- tutorial completion marker.

### 2.3 `Home_Menu`

Purpose:

- main navigation hub,
- mode selection,
- profile summary,
- daily rewards,
- resume prompt.

Must contain:

- Quick Match entry,
- Nets entry,
- Career entry,
- online entry,
- customize/settings,
- animated background,
- ambient audio loop.

### 2.4 `Tutorial`

Purpose:

- onboarding flow,
- batting lesson,
- bowling lesson,
- fielding lesson,
- camera learning.

Must contain:

- tutorial step controller,
- practice prompts,
- fail/pass feedback,
- skip/return logic,
- camera overlay prompts.

### 2.5 `Nets`

Purpose:

- first playable training mode,
- batting practice,
- bowling practice,
- camera testing,
- input tuning.

Must contain:

- batting lane,
- bowling machine or AI bowler,
- practice HUD,
- timing feedback,
- camera toggles,
- reset/retry flow.

### 2.6 `Match_Setup`

Purpose:

- select team,
- choose roles,
- pick camera,
- pick assists,
- choose difficulty,
- load match data.

Must contain:

- squad selection,
- batting order editor,
- camera chooser,
- assist settings,
- format chooser,
- confirmation summary.

### 2.7 `Match_Play`

Purpose:

- full real-time cricket gameplay,
- ball-by-ball flow,
- fielding, batting, bowling, rules, score updates.

Must contain:

- match HUD,
- score overlay,
- camera controller,
- gameplay state controller,
- fielding handoff system,
- pause menu.

### 2.8 `Replay`

Purpose:

- highlight playback,
- wicket replay,
- boundary replay,
- close call review.

Must contain:

- replay timeline,
- camera switching,
- slow motion controls,
- skip/exit controls.

### 2.9 `Results`

Purpose:

- show match outcome,
- awards,
- stats,
- progression,
- unlocks.

Must contain:

- score summary,
- player performance summary,
- reward display,
- next action buttons.

### 2.10 `Customize`

Purpose:

- cosmetics,
- profile identity,
- camera defaults,
- controls,
- accessibility.

Must contain:

- loadout screens,
- camera settings,
- control sensitivity,
- visual settings,
- save/apply behavior.

### 2.11 `Online_Lobby`

Purpose:

- multiplayer matchmaking,
- lobby readiness,
- team assignment,
- session start.

Must contain:

- connection state,
- lobby list or invite flow,
- ready states,
- ping or region info,
- fallback to offline if needed.

## 3. Additive Scene Loading Model

Use this model:

- bootstrap scene loads first,
- main menu loads next,
- gameplay scenes load additively,
- UI overlay scene can remain persistent,
- replay and results can be loaded as needed,
- heavy optional assets stay in Addressables.

This keeps mobile memory use under control.

## 4. Persistent Systems

These systems should survive across scenes:

- audio manager,
- save manager,
- profile manager,
- remote config manager,
- analytics manager,
- network/session manager,
- camera settings manager,
- UI root,
- loading overlay.

These should live in a persistent bootstrap object or persistent service container.

## 5. Scene Responsibilities by Layer

### 5.1 Presentation Layer

- camera rigs,
- lights,
- visual effects,
- UI,
- cinematic transitions.

### 5.2 Gameplay Layer

- batting,
- bowling,
- fielding,
- rules,
- match flow,
- AI backfill.

### 5.3 Meta Layer

- progression,
- unlocks,
- profile save,
- config,
- analytics,
- event hooks.

## 6. Camera and Scene Setup

Each scene should know which camera modes it allows.

### `Boot`

- no gameplay camera,
- only splash and loading visuals.

### `Home_Menu`

- animated menu camera,
- ambient stadium view,
- profile card focus camera.

### `Tutorial`

- training camera,
- guided camera cut-ins,
- optional FPP or broadcast teaching view.

### `Nets`

- `FPP / Batter View`
- `Batting Broadcast Camera`
- `Bowling End Camera`
- `Top-Down Tactical Camera`

### `Match_Play`

- `Batting Broadcast Camera`
- `Bowling End Camera`
- `Mid-Wicket Tactical Camera`
- `Chase Camera`
- `Top-Down Tactical Camera`

### `Replay`

- `Replay Camera`
- cinematic cut cameras
- slow-motion closeups

### `Results`

- presentation camera,
- reward closeup camera,
- stat reveal camera.

## 7. Scene Setup Details

### 7.1 Boot Scene

- very small initial asset load,
- version and services initialization,
- progress UI,
- error fallback UI.

### 7.2 Menu Scene

- root UI canvas,
- profile header,
- animated background stadium,
- music and ambient loops,
- navigation state machine.

### 7.3 Tutorial Scene

- lesson controller,
- helper prompts,
- target objects,
- fail/retry loops,
- success gates.

### 7.4 Nets Scene

- practice pitch,
- ball spawn or machine,
- batting lane,
- input feedback,
- reset buttons,
- camera switch shortcuts.

### 7.5 Match Scene

- stadium,
- pitch,
- boundary objects,
- players,
- score HUD,
- rules engine bindings,
- crowd and presentation systems.

### 7.6 Replay Scene

- timeline player,
- replay camera,
- overlay stats,
- next/prev clip control.

### 7.7 Results Scene

- summary cards,
- rewards,
- unlocks,
- next action buttons.

## 8. Production Order

### Phase 1

- Boot scene
- Home menu scene
- simple profile load

### Phase 2

- Tutorial scene
- Nets scene
- first camera stack

### Phase 3

- Match setup scene
- Match play scene
- results scene

### Phase 4

- Replay scene
- Customize scene
- online lobby scene

### Phase 5

- persistence and service hardening
- loading performance optimization
- addressable content splitting

## 9. AI Tool Usage per Scene

### Best Uses

- `Claude Code`: scene logic, service containers, UI flow code
- `Unity MCP`: direct scene editing, object inspection, component wiring
- `Cursor`: quick UI and script iteration
- `Codex CLI`: validation, batch refactors, repeatable checks

### AI Workflow

1. Define the scene goal in text.
2. Create the scene scaffold.
3. Add persistent services.
4. Add UI flow.
5. Add gameplay objects.
6. Connect the scene to the domain layer.
7. Test loading and transition behavior.

## 10. Acceptance Criteria

The scene system is ready when:

- the app starts in a lightweight boot flow,
- the player can reach a match quickly,
- the tutorial can teach core mechanics,
- gameplay scenes load cleanly,
- camera modes work per scene,
- replay and results are separate from match simulation,
- mobile memory use stays controlled,
- multiplayer can be added without redesigning the whole scene structure.

# Milestone Checklist

This checklist is the execution companion to the master implementation plan. It contains only milestones, deliverables, and exit criteria.

## Milestone 0: Documentation and Project Foundation

### Deliverables

- Complete doc set in `files/`
- Root `README.md`
- `CONTRIBUTING.md`
- `files/INDEX.md`
- Unity project folder structure
- Initial Unity package selection
- AI workflow and MCP setup
- Base Git workflow and branching rules

### Exit Criteria

- A new contributor can understand the project from the docs.
- The Unity project opens with the expected structure.
- The AI toolchain is ready for use.

## Milestone 1: Project Initialization

### Deliverables

- Unity 6 LTS project created
- URP configured
- Bootstrap scene created
- Persistent service container created
- Basic loading and fallback flow
- Placeholder menu and gameplay scenes

### Exit Criteria

- The project boots consistently.
- Scenes can load without manual repair.
- The editor and build target both run cleanly.

## Milestone 2: Data and Service Layer

### Deliverables

- Mode config loading
- Player profile data
- Ground and pitch data
- Camera config data
- Progression data
- Local save service
- Remote config integration
- Analytics hooks
- Session controller

### Exit Criteria

- Data loads correctly from authored assets.
- Save and load work reliably.
- Services are reusable across scenes.

## Milestone 3: Animation Pipeline

### Deliverables

- Shared player rig standard
- Base locomotion clips
- Batting stance and shot clips
- Bowling run-up and delivery clips
- Fielding pickup, throw, and catch clips
- Wicketkeeper clips
- Animator Controllers and blend trees
- Animation Rigging setup
- Timeline support for cinematic sequences

### Exit Criteria

- Cricket actions are readable and retarget correctly.
- Batting, bowling, and fielding have usable motion states.
- Animation can be tested in Unity without scene hacks.

## Milestone 4: Scene Pipeline

### Deliverables

- Boot scene
- Login/Profile scene
- Home/Menu scene
- Tutorial scene
- Nets scene
- Match Setup scene
- Match Play scene
- Replay scene
- Results scene
- Customize scene
- Online Lobby scene

### Exit Criteria

- The app flow works from boot to menu to play to results.
- Each scene has a single clear responsibility.
- Scene transitions are fast and stable on mobile.

## Milestone 5: Core Gameplay Loop

### Deliverables

- Ball simulation
- Batting timing windows
- Shot selection and timing outcomes
- Bowling execution and release logic
- Fielding control handoff
- Wicket and extras logic
- Over and innings flow
- Score updates
- Result resolution

### Exit Criteria

- A solo match can start and finish.
- Rules resolve correctly.
- The game feels like cricket, not a generic sports prototype.

## Milestone 6: Mobile MVP

### Deliverables

- Touch-first controls
- Tutorial/onboarding flow
- Quick Match
- Nets as first playable
- Offline play
- Save/resume
- Mobile camera modes
- Performance tiers
- Mobile HUD and menus

### Exit Criteria

- A new player can learn quickly.
- A short match is playable on a phone.
- The game remains usable offline.

## Milestone 7: Multiplayer Readiness

### Deliverables

- Local/LAN multiplayer proof
- AI backfill behavior
- Control handoff under latency
- Disconnect/reconnect handling
- Session ownership flow
- Match state replication foundation

### Exit Criteria

- Small multiplayer matches are reliable.
- Multiplayer does not require reworking the single-player architecture.

## Milestone 8: Main Mode Scale-Up

### Deliverables

- 11v11 support
- Full physics fidelity tier
- Full rules engine
- Interest management
- Bandwidth and performance optimization
- Dedicated server readiness or networking escalation decision

### Exit Criteria

- Full innings complete online.
- Performance remains acceptable at scale.
- Rules remain correct under network stress.

## Milestone 9: Progression and Live Features

### Deliverables

- Rewards and unlocks
- Cosmetics
- Missions and challenges
- Replay and highlight flow
- Seasonal or live content hooks
- Analytics-driven tuning

### Exit Criteria

- Retention systems work without hurting fairness.
- Live content can be added without code churn.

## Milestone 10: Polish and Expansion

### Deliverables

- Camera polish
- Presentation polish
- Animation polish
- Additional content variants
- Additional mode variants
- Accessibility refinements

### Exit Criteria

- The game is stable, coherent, and ready for broader expansion.
- New content can be added through the data-driven pipeline.

## Usage Rule

Do not start a later milestone until the previous milestone’s exit criteria are met.

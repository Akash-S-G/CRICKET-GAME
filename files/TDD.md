# Technical Design Document (TDD)

## 1. Engine & Version

- Unity 6 LTS (exact patch pinned in `ProjectSettings/ProjectVersion.txt` and Feature 00 before implementation)
- Render pipeline: URP (better mobile performance for Gully/MinBoundary; sufficient fidelity for Main Mode on PC/console)
- Runtime UI: uGUI; UI Toolkit is editor-only unless a feature doc explicitly overrides this.
- Animation packages: Animation Rigging `1.2.x`, Cinemachine `3.0.x`, Timeline from the pinned Unity release.
- Networking: Netcode for GameObjects version pinned in `Packages/manifest.json`.

## 2. High-Level Architecture

```
                 ┌───────────────────────┐
                 │   Authoritative Server  │
                 │  (dedicated or host)    │
                 │                         │
                 │  - Ball physics sim     │
                 │  - Rules/Umpire engine  │
                 │  - Controller manager   │
                 │    (Human/AI handoff)   │
                 └───────────┬─────────────┘
                              │ NGO replication
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
   ┌────▼────┐          ┌────▼────┐          ┌────▼────┐
   │ Client 1 │          │ Client 2 │   ...    │ Client N │
   │ (predict │          │ (predict │          │ (predict │
   │  + recon)│          │  + recon)│          │  + recon)│
   └─────────┘          └─────────┘          └─────────┘
```

**Ball physics MUST be server-authoritative.** Client-side prediction is used only to hide latency for the locally-controlled player's own actions (bat swing timing, fielder movement); the server's resolved ball trajectory is always the source of truth, reconciled back to clients each tick.

## 3. Networking Model

- **Tick rate:** target 30Hz simulation tick minimum for ball physics (fast-moving ball needs tighter tick than typical 3rd-person games); evaluate 60Hz if bandwidth/perf allows during the 2v2 milestone.
- **Client-side prediction + reconciliation:** each client predicts its own controlled player's movement locally, server corrects via periodic authoritative snapshots; standard NGO reconciliation pattern.
- **Interest management:** do NOT replicate full-fidelity transform data for all 22 players to every client every tick. Cull/reduce update rate for players far from the ball and outside the camera's likely view. Revisit this specifically at the Main Mode milestone — it's the single biggest bandwidth risk.
- **Handoff sync:** control-transfer events (AI→Human, Human→AI) must be sent as explicit authoritative RPCs from server, not inferred client-side, to avoid desync on who currently "owns" a fielder.
- **Animation replication:** replicate gameplay animation intent and authoritative event IDs, not Animator transforms or every Animator parameter. Replicated intent includes `shotType`, `timingQuality`, `shotPower`, `footworkContext`, `deliveryType`, `releaseQuality`, and `controllerState`.
- **Animation ownership:** the server owns the resolved contact/release/catch event and `deliveryId`; each client owns local Animator evaluation and presentation. A client must not resolve a hit because a local animation event fired.

## 4. Controller Abstraction

```csharp
public interface IPlayerController
{
    PlayerRole Role { get; }
    ControllerState State { get; } // AI, Human, TransitioningToHuman, TransitioningToAI
    void OnInputTick(PlayerInputFrame input);      // human input path
    void OnAITick(WorldSnapshot snapshot);          // AI decision path
    Vector3 GetPredictedPosition(float futureTime);
    void RequestControlHandoff(ControllerHandoffRequest request);
    void OnHandoffConfirmed(ControllerOwner newOwner, PhysicalState continuityState);
}

public readonly struct PhysicalState
{
    public Vector3 Position { get; init; }
    public Vector3 Velocity { get; init; }
    public Quaternion Rotation { get; init; }
    public string AnimationStateId { get; init; }
    public float AnimationNormalizedTime { get; init; }
    public uint ServerTick { get; init; }
    public ulong DeliveryId { get; init; }
}

public class HumanController : IPlayerController { /* reads local/replicated input */ }
public class AIController : IPlayerController { /* behavior tree / utility AI */ }
```

Both implementations drive the same `PlayerPhysicsBody` and `PlayerAnimator` — the rest of the game (netcode, animation, UI) queries `IPlayerController.State`, never checks "is this human" directly. Full handoff logic: `controller_handoff_spec.md`.

`TransitioningToHuman` and `TransitioningToAI` use a `0.15 s` presentation blend. During the blend, the server owner is already authoritative; the blend only prevents a visible snap. The receiving Animator initializes from `PhysicalState.AnimationStateId` and `PhysicalState.AnimationNormalizedTime`.

## 5. Physics Fidelity Tiers

Driven by `ModeConfig.physics_fidelity_tier`:

| System | Full (Main Mode) | Simplified (Gully/MinBoundary) |
|---|---|---|
| Swing model | Full aerodynamic coefficient calc (seam position, air pressure differential) | Fixed simplified swing curve, no seam-position nuance |
| Pitch interaction | Per-pitch-type bounce/seam/spin modifiers, wear curve over overs | Flat/static bounce and turn, no wear simulation |
| Collision solver | Higher iteration count, sub-stepping for bat-ball contact | Fewer iterations, acceptable visual approximation |
| Fielding AI | Full positional/tactical logic | Simplified reactive-only logic |
| Animation | Full batting/bowling/fielding variants, keeper layer, IK correction | Priority 1 clips only, no keeper detail layer, reduced IK passes, no non-critical dives |

Animation and physics tiers are independent settings but must be selected from the same `ModeConfig.physics_fidelity_tier` unless a documented device fallback lowers presentation quality only.

## 6. Tick and Animator Update Rules

- Authoritative ball and contact simulation runs at fixed `30 Hz` with sub-stepping for fast bat-ball contact.
- Gameplay-critical input intent is sampled at the simulation tick and carries a client timestamp plus input sequence.
- Animator evaluation runs in `Normal` update mode at the render rate for responsive bat motion.
- Animation events are presentation cues only. Server resolution uses the normalized event window and authoritative simulation tick.
- Bat contact uses a normalized contact window of `0.40 +/- 0.04` for a 30-frame reference clip; the exact authored frame is recorded per clip but resolves through the delivery timing contract.

## 7. Data-Driven Design

All tunable values (player stats, pitch behavior, mode rules, physics constants) live in `/data` as JSON, authored/mirrored via Unity ScriptableObjects for in-editor convenience. See `data/*_schema.json` files. New modes should be addable via a new `ModeConfig` asset — **not** new code — once the mode system is built.

## 8. Project Folder Structure

```text
/Assets/_Project
  /Scripts
    /Core                  (shared domain and runtime contracts)
    /Gameplay/Physics      (ball flight, bat-ball collision, pitch interaction)
    /Gameplay/Controllers  (IPlayerController + Human/AI implementations)
    /Gameplay/Rules        (state machine, umpire logic, LBW/DRS)
    /Networking            (NGO setup, replication, handoff RPCs)
    /Services              (save, profile, analytics, remote config)
    /Animation             (Animator, rigging, animation event adapters)
  /Modes                   (Main / Gully / MinBoundary / Nets)
  /Data                    (Players / Pitches / Grounds / ModeConfigs)
  /Animation               (controllers, clips, masks, rigging assets)
  /UI
/data                      (source-of-truth JSON schemas + values, mirrored into ScriptableObjects)
/docs                      (this document set)
```

## 8. Save/Session Data

- **Persistent:** player profiles, match history, stats, cosmetic unlocks — stored server-side (or cloud save), synced on login.
- **Ephemeral (in-match only):** ball state, current over/innings state, live field placement — exists only in the active session, discarded on match end except for the summary stats that get persisted.

## 9. Open Technical Risks

See `risks.md` for the full register; top three to resolve early via prototyping:
1. NGO bandwidth/perf at 22 concurrent players with fast ball physics
2. Control-handoff visual smoothness under realistic latency (100–150ms)
3. LBW prediction accuracy (ball-tracking projection through to stumps)

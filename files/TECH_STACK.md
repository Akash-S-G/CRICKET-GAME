# Tech Stack Decision

## Engine: Unity 6 LTS (C#)

**Why, given heavy reliance on AI coding agents / MCP:**

- Unity ships an official **Unity MCP Server** (Unity AI open beta) giving Claude Code, Cursor, and other IDE agents live access to scene hierarchy, GameObjects, component values, console output, and the ability to edit scripts and trigger Editor actions directly — no manual copy-pasting of context between editor and agent.
- Mature community MCP servers exist as alternatives/supplements (e.g. `IvanMurzak/Unity-MCP` — 71 tools, 46 prompts, explicitly lists Claude Code as "highly recommended"; `CoderGamester/mcp-unity`). This means you have redundancy/choice in tooling, not a single fragile integration.
- C# is dramatically more agent-friendly than Unreal's C++/Blueprints combo: far more training data, cleaner single-responsibility file conventions, no visual-scripting-to-text mismatch. Agents write, review, and refactor C# more reliably.
- Unity's component/prefab/ScriptableObject model maps naturally onto the kind of small, well-scoped files coding agents are best at generating and editing incrementally — this matters a lot for the data-driven architecture this project needs (mode configs, player stats, pitch types as data assets).
- Faster iteration loop (domain reload, play-in-editor) means an agent-driven "generate → test → fix" cycle is faster than Unreal's compile times.

**Trade-off accepted:** Unreal's Chaos physics and out-of-box mocap/animation tooling are more powerful for high-end visual fidelity. Given the priority here is agent-assisted development velocity, Unity is the right call — Unity's physics is entirely sufficient for a well-engineered custom cricket ball/bat simulation (you're writing custom aerodynamics code either way, not relying on the engine's default physics for swing/seam).

## Networking: Unity Netcode for GameObjects (NGO), server-authoritative

- Official Unity package, actively maintained, well documented — meaning agents have strong reference material and NGO's API surface is well represented in training data.
- Supports server-authoritative simulation with client-side prediction and reconciliation via `NetworkTransform`/custom prediction — required given ball-physics lag sensitivity.
- **Escalation path if NGO's prediction model proves insufficient for 22-player fast-ball-physics scale:** Photon Fusion 2 is the fallback — also C#, built specifically for competitive/physics-heavy multiplayer with predicted/authoritative modes. Don't start here; NGO is simpler to scaffold with agent assistance and is likely sufficient through the 2v2/Gully milestones. Re-evaluate before the full 22-player Main Mode milestone.

## Supporting tools

- **IDE/Agent:** Claude Code connected via Unity MCP, for direct in-editor script generation, scene inspection, and console-log-driven debugging loops.
- **Version control:** Git + Git LFS (Unity binary assets). Structure commits so agent-generated changes are reviewable in small diffs — avoid letting an agent touch dozens of files in one pass early on.
- **Data format:** ScriptableObjects for in-editor authoring convenience, backed by JSON schemas (see `/data`) for the actual source-of-truth values — this lets an agent (or a human) edit balance/tuning data as plain text/JSON without needing the Unity Editor open.
- **CI:** Unity Cloud Build or self-hosted, for automated build verification — important once agents are making frequent commits, so regressions get caught fast.

## Platform targets

- **Main Mode:** PC/console first (networking + physics fidelity ceiling higher, less constrained by mobile hardware).
- **Gully / MinBoundary:** Mobile-first design (simplified physics tier), same Unity project — no separate codebase.

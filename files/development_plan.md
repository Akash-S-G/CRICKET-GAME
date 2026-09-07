# Mobile Cricket Game: AI Tooling and Development Roadmap

## Summary
Build the game in Unity 6 LTS with a single primary AI coding stack, a small set of MCP servers, and a milestone-gated roadmap. The key decision is to optimize for mobile-first play, fast iteration, and editor-aware tooling so the team can move from docs to playable slices without manual context copying.

## Recommended Tool Stack

- Primary coding agent: `Claude Code`
- Unity editor bridge: `Unity MCP Server`
- Secondary IDE agent: `Cursor` for in-editor edits and quick cross-file changes
- Terminal automation: `Codex CLI` for repo ops, scripted refactors, test runs, and fast shell workflows
- Docs grounding: `Context7 MCP` for up-to-date framework/library docs when the agent needs current API references
- Source control workflow: Git + small reviewable commits, with the AI agent always working from the repo docs as the source of truth

## How to Use the Tools

- Use `Claude Code + Unity MCP` for architecture work, gameplay systems, and Unity Editor-aware changes.
- Use `Cursor + Unity MCP` for rapid iteration on scripts, UI, and scene-level debugging.
- Use `Codex CLI` for repeatable terminal tasks, test execution, and batch maintenance work.
- Use `Context7 MCP` only when the agent needs current external documentation for a dependency or API.
- Keep one primary agent responsible for each change set to avoid conflicting edits.

## Development Roadmap

### Phase 0: Tooling and Project Setup

- Settle the Unity project structure, repo conventions, and AI workflow rules.
- Connect Unity MCP and verify the editor can be inspected and controlled.
- Create a small set of operating docs for agent usage, branching, and review.
- Lock the initial technical stack: Unity 6 LTS, URP, NGO, server-authoritative simulation.

### Phase 1: Physics Prototype

- Implement ball flight, swing, seam, pitch bounce, and bat-ball collision.
- Ignore networking and polish; use primitive visuals only.
- Exit when timing, swing, and contact produce believable cricket outcomes.

### Phase 2: Nets Mode

- Build the first playable batting loop with AI bowler or bowling machine.
- Add the first camera set: `Batting Broadcast` and `FPP / Batter View`.
- Add batting tutorial hooks and basic timing feedback.
- Exit when a new player can learn batting quickly and hit varied shots.

### Phase 3: Core Gameplay Systems

- Add bowling controls, fielding handoff, and controller abstraction.
- Build the rules engine for wickets, extras, innings flow, and score state.
- Add the remaining camera modes: `Bowling End`, `Top-Down Tactical`, `Chase`, `Replay`.
- Exit when a solo human-vs-AI match can start and finish cleanly.

### Phase 4: Mobile MVP

- Add touch-first controls, short-session match flow, save/resume, offline play, and low-end performance tiers.
- Make Quick Match and Nets the default mobile entry points.
- Add the onboarding/tutorial flow so first-time users can learn in under a few minutes.
- Exit when the game is playable, understandable, and stable on a phone.

### Phase 5: Multiplayer Gully

- Add NGO-based online play for small matches first.
- Validate AI backfill, handoff behavior, and latency smoothing.
- Exit when partial lobbies still produce a complete, reliable match.

### Phase 6: Main Mode

- Scale to 11v11, full rules, and full interest management.
- Re-evaluate NGO versus Photon Fusion only here, not earlier.
- Exit when a full innings completes online with correct rules and acceptable performance.

### Phase 7: Progression and Live Features

- Add career progression, cosmetics, events, missions, challenges, and replay/highlights.
- Keep progression non-pay-to-win and mobile-session friendly.
- Exit when the meta loop gives players reasons to return without harming competitive integrity.

## Test Plan

- Physics tests for timing windows, swing, seam, pitch behavior, and contact outcomes.
- Tutorial tests for first-launch completion, batting, bowling, fielding, and camera learning.
- Mobile tests for UI readability, battery/load behavior, low-end performance, save/resume, and offline play.
- Multiplayer tests for AI backfill, handoff smoothness, disconnect/reconnect, and run-out/stumping authority.
- Main Mode tests for full innings completion, rules correctness, and network stability.
- Camera tests for each mode default and switching behavior.

## Assumptions

- The game remains Unity-first and mobile-first.
- One primary AI agent owns each feature slice.
- Docs stay current and are treated as the source of truth.
- AI tools accelerate implementation, but they do not replace milestone gating or manual review.

## References

- [Unity MCP overview](https://docs.unity3d.com/Packages/com.unity.ai.assistant%402.0/manual/unity-mcp-overview.html)
- [Unity MCP getting started](https://docs.unity3d.com/Packages/com.unity.ai.assistant%402.0/manual/unity-mcp-get-started.html)
- [MCP overview](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro)
- [Cursor MCP docs](https://cursor.com/docs/mcp)
- [Claude Code MCP docs](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Claude Code overview](https://docs.anthropic.com/en/docs/claude-code)
- [Codex CLI docs](https://learn.chatgpt.com/docs/codex/cli)
- [OpenAI CLI docs](https://developers.openai.com/api/docs/libraries/openai-cli)

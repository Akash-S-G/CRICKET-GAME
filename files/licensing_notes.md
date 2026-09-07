# Licensing Notes

- Current scope: **original roster, original team/league names.** No real player names, likenesses, or board/league branding (ICC, BCCI, national team names/kits, etc.) are used.
- If scope ever shifts toward real players/teams/leagues, that requires direct licensing negotiation with the relevant boards/players' associations — a legal/business track entirely separate from development, and should not be assumed as a "later" engineering task. Revisit this document the moment that conversation starts, before any names/likenesses are implemented.
- Third-party asset usage — free/AI-only pipeline per user (no paid subs). Track every clip `Source`/`License` from `animation_clip_inventory.md:1` + sidecar `animation_events_sidecar.json`:

| Clip Group | Source | License | Notes |
|---|---|---|---|
| Locomotion/support `idle_*` `walk_*` `field_*` base | Mixamo (free Adobe account) | Mixamo Free License (non-exclusive, requires Mixamo attribution if redistributed outside Unity) | Record date + URL per clip |
| Signature batting/bowling/keeper `bat_*` `bowl_*` `keeper_*` | Plask Free / MoveAI Free / DeepMotion Free -> Cascadeur Community -> Blender | Custom-AI-Generated / Blender-Custom (user-owned phone video + free tool output) | No Rokoko. Keep raw phone video + FBX + Blender .blend for audit |
| Rigging/cleanup | Animation Rigging 1.3.0 + Blender | Unity Package / GPL-free | Not shipped beyond runtime |
| Support fielding dives `field_dive_*` | Blender Custom | Blender-Custom | Hand-keyed if AI output generic |
- MCP servers / Unity plugins used for development tooling (e.g., community Unity-MCP packages) are development-time dependencies, not shipped in the game build — confirm this remains true (i.e., no MCP server code accidentally bundled into a production build).

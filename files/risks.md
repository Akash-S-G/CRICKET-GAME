# Risk Register

This register is meant to be operational, not descriptive. Each item below should have:

- a known failure mode,
- an early detection signal,
- a concrete mitigation path,
- and a milestone where the risk is either proven safe or replaced with a fallback.

See `risk_resolution.md` for the detailed response plan for each risk.

| # | Risk | Failure mode | Impact | Early warning signs | Mitigation | Resolve by milestone |
|---|---|---|---|---|---|---|
| 1 | NGO bandwidth/perf doesn't scale cleanly to 22 concurrent players with fast ball physics | Replication volume becomes too high, causing delay, missed state updates, or poor client responsiveness | High - could force a networking library switch mid-project | Growing snapshot sizes, frequent correction pops, unstable ball state on remote clients, CPU spikes during overs | Interest management from the start; server-authoritative ball state; separate replication cadence for ball, near-ball players, and distant players; explicit go/no-go before Main Mode; Photon Fusion fallback if required | Milestone 6 |
| 2 | Control-handoff visual smoothness breaks under real-world latency | Human-to-AI or AI-to-human transitions snap, stutter, or feel like input loss | High - directly undermines the core "everyone's playable" pitch | Handoff animations restarting, position pops, control desync between owner and observers | Dedicated latency-injection tests; continuity-state handoff; authority RPCs from server only; short blend windows instead of hard snaps | Milestone 4 |
| 3 | LBW/ball-tracking prediction accuracy is wrong | Ball projection misses edge cases and gives decisions players do not trust | Medium-High - competitive integrity issue in Main Mode | Frequent disputes during test cases, mismatched outcomes in repeated simulations, inconsistent call results on similar trajectories | Pure-function LBW logic with deterministic inputs; unit tests against historical/constructed scenarios; use same trajectory math for simulation and projection | Milestone 6 |
| 4 | Batting animation feel is wrong even when physics is correct | Input timing feels disconnected from animation, making the game look and feel off | Medium-High - feel is make-or-break for cricket | Players timing shots correctly but seeing bad body language, late footwork, or bad blend transitions | Animate Nets mode early; validate timing windows alongside animation states; keep shot types and timing qualities as explicit blend targets | Milestone 2 |
| 5 | Mobile performance for Gully/MinBoundary collapses under shared code | The simplified modes inherit too much simulation cost and become frame-rate limited on mobile | Medium | Mobile builds missing target frame-rate, thermal throttling after short sessions, stutter during fielding or camera motion | Physics-fidelity tiers in `ModeConfig`; limit AI and replication cost in simplified modes; keep mobile-specific tuning isolated | Milestone 7 |
| 6 | Run-out/stumping timing desyncs between clients | Clients disagree about whether batter was in or out when the wicket was broken | Medium | Conflicting replay views, delayed wicket state updates, inconsistent dismissals in network play | Server-authoritative position snapshot at the exact wicket-broken tick; do not derive outcome from client prediction | Milestone 5 |
| 7 | Scope creep pulls focus from the core loop | Career, DRS, weather, presentation, and custom content reduce time for the physics and network backbone | Medium | Milestones slip because feature work keeps bypassing the playability gate | Keep explicit non-goals; only unlock post-core-loop features after match flow passes exit criteria | Ongoing |
| 8 | AI-generated code drifts away from the docs | Implementation diverges from the design and becomes hard to maintain or reason about | Medium | Docs and code disagree, regressions appear after agent edits, repeated rework | Treat docs as source of truth, review deltas against them, and update docs whenever architecture changes intentionally | Ongoing |
| 9 | Community MCP tooling is less stable than official tooling | Tooling failures slow development or produce inconsistent editor behavior | Low-Medium | Tool disconnects, unsupported API changes, flaky editor automation | Prefer official Unity MCP where possible; keep community servers as supplements only | Ongoing |
| 10 | Licensing assumptions leak into gameplay scope | Design drifts toward real teams/players without the legal groundwork | Medium | Requests for real names, logos, stadiums, or league brands start entering feature planning | Keep original-fiction roster/branding until a separate legal track is complete; do not code around licensing gaps | Ongoing |

## Risk Ownership

- Networking and replication risks are tied to Main Mode readiness.
- Handoff and animation risks are tied to Nets and 2-player online readiness.
- Rules accuracy risks are tied to Main Mode and must be testable before launch.
- Licensing and scope risks are governance issues and should be reviewed whenever new content is proposed.

## Acceptance Criteria

The register should only be considered reduced if the following are true:

- The risk can be reproduced or measured in a controlled test.
- A mitigation exists that is realistic for the current milestone.
- A fallback path exists if the mitigation fails.
- The docs and milestone plan both reflect the chosen path.

# Risk Resolution Plan

This document turns `risks.md` into an execution plan. It is intentionally more operational than the register itself.

## 1. Networking Scale

### Risk
22-player online cricket with fast ball physics may overwhelm NGO replication and correction bandwidth.

### What to watch

- Ball state corrections arriving too late to feel stable.
- Fielders far from the ball updating too frequently.
- Remote clients showing visible rubber-banding or stuttering.
- Server tick time climbing under match load.

### Resolution plan

1. Keep the server authoritative for the ball and all dismissals.
2. Replicate full-fidelity state only for actors near the ball or directly relevant to the current phase.
3. Reduce update frequency for distant fielders and spectators.
4. Split the simulation into categories:
   - ball,
   - striker/non-striker,
   - active fielder(s),
   - background fielders.
5. Run a go/no-go test before Main Mode is built:
   - if NGO can hold the target experience, keep it,
   - if not, move to the fallback networking path.

### Exit criteria

- A full 11v11 innings can complete without severe correction popping.
- Packet volume and server frame time stay within target bounds during busy overs.

### Fallback

- Move to Photon Fusion if the bandwidth and prediction model are not adequate at Milestone 6.

## 2. Handoff Smoothness

### Risk
Human-to-AI and AI-to-human fielding handoffs may feel like snapping or lost control under latency.

### What to watch

- Control ownership changing without a visible transition.
- A fielder teleporting a few meters at handoff.
- Input feeling delayed immediately after a handoff.

### Resolution plan

1. Treat handoff as a server-issued event, not a client guess.
2. Always carry the exact `PhysicalState` into the new owner.
3. Blend animation and motion over a short window instead of resetting.
4. Test with artificial latency between 100 and 150 ms.
5. Ensure reconnects happen at natural breaks when possible rather than mid-action.

### Exit criteria

- Handoff looks stable in local and network play.
- Observers see the same ownership transition as the controlling client.

## 3. LBW Accuracy

### Risk
LBW decisions may feel random if the trajectory projection is not trustworthy.

### What to watch

- Repeated identical deliveries producing different results.
- A call changing because of client-side state.
- Edge cases where pitch point, impact point, or shot intent are mishandled.

### Resolution plan

1. Build LBW as a pure function.
2. Use the same physics math for the live ball and the projection.
3. Unit test:
   - pitching outside leg,
   - no-ball exclusions,
   - bat-pad line cases,
   - off-stump impact with shot attempt,
   - same delivery with different impact timing.
4. Keep a small library of known scenarios for regression testing.

### Exit criteria

- The same input always yields the same output.
- The decision tree is documented and covered by tests.

## 4. Batting Feel

### Risk
The game may be mechanically correct but feel wrong because animation and input timing do not align.

### What to watch

- Good timing still producing awkward body motion.
- Early/late timing not visibly changing the swing shape.
- Players not understanding why a shot failed.

### Resolution plan

1. Treat Nets mode as the tuning environment.
2. Build shot animation states around shot type and timing quality, not a single generic swing.
3. Expose the timing window in a way the player can learn.
4. Use animation feedback to teach timing, not just to decorate it.

### Exit criteria

- New players can learn the batting loop in a few minutes.
- Mistimed balls look mistimed and play differently.

## 5. Mobile Performance

### Risk
Gully and MinBoundary may inherit Main Mode complexity and become too expensive for mobile.

### What to watch

- Frame drops during fielding.
- Thermal throttling after short sessions.
- The simplified modes requiring the same asset budget as Main Mode.

### Resolution plan

1. Keep simplified physics as a separate fidelity tier.
2. Reduce AI complexity in casual modes.
3. Reduce replication load and animation density on mobile.
4. Make the mode data drive the budget so the content itself declares the tier.

### Exit criteria

- Gully and MinBoundary hit the target frame-rate on the lowest supported device class.

## 6. Scope Control

### Risk
The project could lose focus if too many "nice to have" features arrive too early.

### What to watch

- Career or DRS work starting before the first playable loop is stable.
- Presentation polish being used to hide physics or network gaps.

### Resolution plan

1. Keep the roadmap order strict.
2. Treat the core loop as the launch gate.
3. Only promote features into active development when they unblock the next milestone.

### Exit criteria

- Every milestone has a testable, narrow definition of done.
- New features do not bypass the milestone sequence.

## 7. Licensing

### Risk
The design could drift into licensed content assumptions without legal approval.

### What to watch

- Real-player requests entering mode or UI design.
- Team, league, and sponsor branding being treated as default.

### Resolution plan

1. Keep the default roster fictional.
2. Keep branding generic until a licensing path is approved.
3. Separate legal/business work from engineering work.

### Exit criteria

- Any licensing-dependent feature is explicitly marked as out of scope until legal approval exists.

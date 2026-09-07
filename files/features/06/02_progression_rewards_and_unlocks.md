# Feature 06.2: Progression, Rewards, and Unlocks

## What This Doc Covers

This doc defines the long-term retention layer for the mobile game.

Progression should reward continued play without overpowering the core cricket experience or making the game feel grindy.

## Scope

Include:

- experience or level progression
- rewards
- unlockable cosmetics or modes
- session goals
- light meta progression

Exclude:

- monetization design
- multiplayer matchmaking
- core match rules
- low-level animation work

## Implementation Tasks

1. Define what progression tracks and why they exist.
2. Keep unlocks mostly cosmetic or convenience-based unless the design explicitly needs otherwise.
3. Add clear reward moments after match completion or milestone achievements.
4. Make sure progression never blocks first play.
5. Document how the progression data flows into save state and profile state.

## Expected Output

- a simple, satisfying progression model
- rewards that encourage repeat sessions
- unlocks that do not destabilize game balance

## Dependencies

- Feature 01 profile and save services
- Feature 06.1 mobile session loop
- results and rewards UI

## References

- [files/mobile_roadmap.md](/home/akash/Desktop/CRICKET/files/mobile_roadmap.md)
- [files/competitive_analysis.md](/home/akash/Desktop/CRICKET/files/competitive_analysis.md)

## AI Agent Tasks

- define progression currency and reward types
- document unlock categories and order
- specify what saves to profile state
- note what must stay optional

## Tests

- progression is awarded correctly
- unlocks persist after restart
- first-time flow is not blocked by progression

## Exit Criteria

- the player sees meaningful rewards over time
- progression stays simple and understandable
- the economy does not interfere with gameplay clarity

# Feature 06.4: Progression Numbers and Unlock Table

## Purpose

This document gives progression implementation fixed values. Progression is a local, fair reward layer. It must not modify batting timing, bowling speed, ball physics, or competitive outcomes.

## Profile Progression Fields

Add these fields to the versioned profile schema from Feature 01:

```text
progressionLevel: int              // starts at 1
progressionXp: int                 // starts at 0
lifetimeRuns: int
lifetimeWickets: int
completedChallenges: string[]
unlockedContentIds: string[]
claimedRewardIds: string[]
dailyState: DailyProgressState
```

`unlockedContentIds` and `claimedRewardIds` must be sets at the domain layer and deterministic arrays in serialized JSON.

## XP Formula

Use integer arithmetic:

```text
xpForNextLevel(level) = 100 + ((level - 1) * 25)
```

Award XP once per completed activity:

| Activity | XP | Limits |
|---|---:|---|
| Complete a Nets session | 20 | 3 awards per day |
| Complete a Quick Match | 60 | 10 awards per day |
| Win a Quick Match | 40 bonus | once per match |
| Score 10 runs in a match | 15 bonus | once per match |
| Take a wicket in a match | 10 bonus | maximum 5 per match |
| Complete the first-run tutorial | 100 | once per profile |
| Complete a daily challenge | 50 | one award per challenge |

XP is granted from domain events, not UI button presses. Reopening a results screen must not grant XP twice.

## Unlock Schedule

| Level | Unlock | Type |
|---:|---|---|
| 1 | Default stadium, batter, and camera set | Mandatory starting content |
| 2 | Bat grip cosmetic set A | Cosmetic |
| 3 | Nets challenge: moving target | Practice content |
| 4 | Stadium lighting preset A | Cosmetic |
| 5 | Bat trail effect A | Cosmetic |
| 6 | Bowler celebration set A | Cosmetic |
| 8 | Nets challenge: swing variation | Practice content |
| 10 | Stadium environment B | Cosmetic/environment |
| 12 | Replay camera filter A | Presentation |
| 15 | Profile badge: Club Regular | Cosmetic |
| 20 | Stadium environment C | Cosmetic/environment |

Unlocking content never increases player attributes or changes simulation constants.

## Daily Challenge Contract

The MVP uses three deterministic challenge templates selected by local date:

- `score_runs`: score 12 runs in any completed session.
- `play_deliveries`: face 18 deliveries.
- `hit_target_zone`: make 3 successful timing outcomes in the target zone.

Each challenge has an ID containing the UTC date, template ID, and version. The same date must produce the same challenge when offline.

## Acceptance Tests

- XP cannot be awarded twice for the same `activityId`.
- Level-up handles multiple levels if a large reward is granted.
- Unlocks persist after restart and schema migration.
- No progression reward changes a gameplay constant.
- Daily challenges remain stable offline for the device date policy.
- A fresh profile can play all core modes without unlocking anything.

## Dependencies

- `features/01/00_profile_field_list_and_versions.md`.
- `features/01/02_profile_save_session_services.md`.
- `features/03/03_match_hud_replay_and_results.md`.
- Feature 04 domain events.

## Expected Outputs

- `ProgressionService` backed by domain events.
- Versioned reward and unlock data assets.
- Idempotency tests for activity rewards.
- Results screen reward presentation.


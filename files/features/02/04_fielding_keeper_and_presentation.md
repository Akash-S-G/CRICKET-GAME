# Feature 02.4: Fielding, Keeper, and Presentation Motion

## What This Doc Covers

This doc groups the supporting cricket motions outside batting and bowling.

These motions are important because they make the game feel alive, especially during catches, saves, appeals, celebrations, and replays.

## Scope

Include:

- fielding run, pickup, throw, dive, catch, and miss motion
- wicketkeeper crouch, take, stumping, and appeal motion
- umpire signals
- celebrations and reactions
- replay and intro presentation motion

Exclude:

- batting-specific motion
- bowling-specific motion
- rules behavior
- UI layout

## Implementation Tasks

1. Build fielding movement states for chasing, stopping, sliding, diving, throwing, and recovering.
2. Create keeper-specific takes for low balls, high balls, stumpings, and appeals.
3. Add catch and near-miss variants so outcomes do not all look identical.
4. Create celebration and frustration states that respond to match events.
5. Add umpire gestures for common dismissal and boundary decisions.
6. Define motion hooks for replay camera closeups and presentation sequences.

## Expected Output

- richer match presentation
- clear visual feedback for fielding outcomes
- keeper behavior that feels special instead of generic

## Dependencies

- Feature 02.1 rig and locomotion
- Feature 04 fielding and dismissal logic
- scene and replay system

## References

- [files/animation_clip_inventory.md](/home/akash/Desktop/CRICKET/files/animation_clip_inventory.md)
- [files/scene_by_scene_setup.md](/home/akash/Desktop/CRICKET/files/scene_by_scene_setup.md)

## AI Agent Tasks

- create the fielding motion matrix
- define keeper-specific clip requirements
- map match events to celebration states
- document replay and intro motion usage

## Tests

- fielders show readable intent
- keeper takes line up with ball height and speed
- presentation motion can be triggered without gameplay bugs

## Exit Criteria

- the game has complete support motion for cricket moments
- fielding and keeper behavior are easy to understand visually
- presentation motion improves match feel

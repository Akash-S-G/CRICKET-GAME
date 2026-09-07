# Feature 05.0: Action Maps and Gesture Thresholds

## Purpose

This doc fixes the player interaction model before input code is implemented.

## Action Map List

- UI
- Batting
- Bowling
- Fielding
- Camera

## Gesture Thresholds

Use these initial thresholds:

- tap: under `150 ms`
- long press: over `500 ms`
- swipe minimum distance: `40 px`
- intentional swipe distance: `120 px`
- aim drag dead zone: `8 px`
- timing assist window: `180 ms`

## Sensitivity Ranges

- camera sensitivity: `0.5` to `2.0`
- batting timing assist: `0.0` to `1.0`
- swipe sensitivity: `0.5` to `1.5`

## Mapping Rules

1. Touch and controller should map to the same gameplay intent.
2. UI actions must not leak into gameplay when match input is active.
3. Gestures should be deterministic and limited in number.
4. Camera actions should never override a live batting timing input.

## Required Deliverable

Create the Unity Input Actions asset before any gameplay slice that reads input.

## Exit Criteria

- every core action has a concrete binding home
- thresholds are documented
- AI agents cannot invent new control semantics accidentally


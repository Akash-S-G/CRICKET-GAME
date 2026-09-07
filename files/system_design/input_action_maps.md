# Input Action Maps

## Purpose

This doc locks the input architecture so the game can support mobile touch and controller input without feature-specific hacks.

## Required Package Choice

- Use Unity Input System.
- Do not build a custom input polling layer on top of legacy input.

## Action Maps

### 1. UI

Used in menus and non-match screens.

Actions:

- `Navigate`
- `Submit`
- `Cancel`
- `Back`
- `Pause`
- `Select`

### 2. Batting

Actions:

- `AimDirection`
- `ShotIntent`
- `ShotPower`
- `Defend`
- `TapTiming`
- `SprintBetweenWickets`
- `CameraToggle`

### 3. Bowling

Actions:

- `LineAim`
- `LengthAim`
- `DeliveryType`
- `ReleaseTiming`
- `SpinVariation`
- `RunUpConfirm`

### 4. Fielding

Actions:

- `Move`
- `Sprint`
- `Dive`
- `PickUp`
- `Throw`
- `Relay`
- `Catch`

### 5. Camera

Actions:

- `CameraNext`
- `CameraPrev`
- `CameraReset`
- `CameraMode1`
- `CameraMode2`
- `CameraMode3`

## Touch Rules

Touch gestures must be explicit and limited:

- tap = confirm or timing action
- hold = power buildup or aim lock
- swipe horizontal = direction change
- swipe vertical = length or elevation change
- drag = continuous aim control

## Gesture Thresholds

Use consistent thresholds across the whole game:

- minimum swipe distance: `40 px`
- intentional swipe distance: `120 px`
- hold start threshold: `150 ms`
- long press threshold: `500 ms`
- tap timing window: mode-tunable, default `180 ms` around the target frame

## Controller Rules

- Controller should map to the same action model as touch.
- Input names must be identical across device types.
- Gameplay code should read actions, not device types.

## Input Routing

Routing order:

1. UI when menus are active.
2. Gameplay action map when match state is interactive.
3. Camera map only when the camera is not locked by an event.

## Dependencies

- Feature 05 input doc
- camera and gameplay state machines
- UI navigation stack

## Tests

- touch gestures trigger the correct actions
- controller and touch produce the same gameplay intent
- UI never consumes gameplay input during match play

## Exit Criteria

- every player action has an action-map home
- touch thresholds are documented and testable
- input code does not depend on scene-specific logic


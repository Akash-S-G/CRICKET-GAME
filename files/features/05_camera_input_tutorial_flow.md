# Feature 05: Camera, Input, and Tutorial Flow

## 1. What This Feature Is

This feature implements the usability layer of the game.

It includes:

- camera modes,
- touch input mapping,
- controller input mapping,
- onboarding,
- tutorial flow,
- first-run learning,
- return-player guidance,
- accessibility and assist settings.

## 2. Why This Feature Matters

This feature determines whether the player can actually understand and enjoy the game.

Even if the simulation is good, the game will fail if:

- the player cannot read the action,
- the inputs are confusing,
- the tutorial is too long,
- the camera is wrong,
- the player cannot recover after returning to the game later.

## 3. Camera System

### 3.1 Required Camera Modes

Implement:

- `FPP / Batter View`
- `Batting Broadcast Camera`
- `Bowling End Camera`
- `Mid-Wicket Tactical Camera`
- `Chase Camera`
- `Top-Down Tactical Camera`
- `Replay Camera`

### 3.2 Camera Behavior

- each mode should have sensible defaults,
- camera switching should be fast,
- mobile controls should not be blocked by camera changes,
- the camera should support clarity before spectacle when needed.

### 3.3 Camera Tasks

- build camera presets,
- build camera switching,
- build camera defaults per mode,
- build camera UI for settings,
- make camera transitions smooth,
- make replay cameras suitable for highlights.

## 4. Input System

### 4.1 Touch Controls

Touch controls must support:

- batting direction,
- batting timing,
- power input,
- bowling release,
- fielding movement,
- dive/throw triggers,
- camera and UI interactions.

### 4.2 Controller Mapping

Controller support should map to the same gameplay concepts:

- footwork,
- shot direction,
- shot type,
- bowling choice,
- fielding control,
- menu navigation.

### 4.3 Input Configuration

The player should be able to adjust:

- sensitivity,
- camera preference,
- assist level,
- touch feel,
- controller mapping where needed.

## 5. Tutorial Design

### 5.1 Tutorial Order

Teach in the order a player needs to understand the game:

1. basic movement and camera
2. batting
3. bowling
4. fielding
5. match setup
6. progression
7. practice use

### 5.2 Tutorial Tasks

The tutorial should give the player actual actions to complete:

- defend a ball,
- score a run,
- bowl a target ball,
- complete a fielding action,
- change camera,
- enter a match,
- view rewards.

### 5.3 Tutorial UX Rules

- keep prompts short,
- show one lesson at a time,
- use live gameplay instead of text-only explanation,
- make the tutorial skippable for returning players,
- allow the player to jump into Nets or Quick Match quickly.

## 6. Expected Output

- New players can learn quickly.
- Camera choices are meaningful.
- Input feels usable on mobile and controller.
- The tutorial gets the player into real play without confusion.

## 7. Dependencies

- Feature 02 animation pipeline
- Feature 03 scene pipeline
- Feature 04 gameplay
- `game_flow_and_camera_design.md`
- `player_onboarding_and_tutorial_flow.md`
- `mobile_roadmap.md`

## 8. References

- [game_flow_and_camera_design.md](../game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](../player_onboarding_and_tutorial_flow.md)
- [mobile_roadmap.md](../mobile_roadmap.md)
- [animation_requirements.md](../animation_requirements.md)

## 9. AI Agent Tasks

An AI agent working on this feature should:

- build the camera mode controller,
- build the input abstraction,
- build the tutorial state machine,
- build tutorial objectives,
- build camera settings UI,
- build input settings UI,
- build onboarding validation tests.

## 10. Expected Tests

- tutorial completion tests,
- camera switching tests,
- touch input response tests,
- controller input mapping tests,
- settings persistence tests,
- return-player skip path tests.

## 11. Exit Criteria

- The player can complete the tutorial.
- The camera system is functional.
- The controls are understandable.
- The game can onboard a first-time user.

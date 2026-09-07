# Feature 05: Camera, Input, and Tutorial Flow

## 1. What This Feature Is

This feature implements:

- the camera modes,
- touch input mapping,
- controller input mapping,
- onboarding,
- tutorial flow,
- first-run learning,
- return-player guidance.

## 2. How To Implement It

### 2.1 Camera Modes

- Implement the documented camera types.
- Provide default cameras per mode.
- Allow fast switching.

### 2.2 Input Mapping

- Map touch controls to batting, bowling, and fielding.
- Keep controller mapping parallel to the same gameplay actions.
- Make input configurable.

### 2.3 Tutorial Flow

- Teach batting first.
- Then bowling.
- Then fielding.
- Then camera modes.
- Then match setup and progression.

## 3. Expected Output

- New players can learn quickly.
- Camera choices are meaningful.
- Input feels usable on mobile and controller.

## 4. Dependencies

- Feature 02 animation pipeline
- Feature 03 scene pipeline
- Feature 04 gameplay
- `game_flow_and_camera_design.md`
- `player_onboarding_and_tutorial_flow.md`

## 5. References

- [game_flow_and_camera_design.md](../game_flow_and_camera_design.md)
- [player_onboarding_and_tutorial_flow.md](../player_onboarding_and_tutorial_flow.md)
- [mobile_roadmap.md](../mobile_roadmap.md)

## 6. Exit Criteria

- The player can complete the tutorial.
- The camera system is functional.
- The controls are understandable.
- The game can onboard a first-time user.

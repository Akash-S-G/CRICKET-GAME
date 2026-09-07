# Feature 03.3: Match HUD, Replay, and Results

## What This Doc Covers

This doc defines the live match interface and the end-of-play presentation flow.

The HUD must keep cricket readable during play, and the replay/results screens must clearly explain what happened after each key moment.

## Scope

Include:

- in-match HUD
- score and over display
- player state indicators
- replay sequence
- wicket and boundary summary
- match results

Exclude:

- boot flow
- tutorial flow
- customization
- online lobby

## Implementation Tasks

1. Design the HUD for quick readability on a phone screen.
2. Show only the cricket information the player needs right now.
3. Build replay entry and exit transitions that do not break match flow.
4. Create results screens that summarize the match in a compact and understandable way.
5. Ensure score changes and wicket events are animated without hiding the main action for too long.

## Expected Output

- a readable live scoreboard
- replay and results screens that explain outcomes cleanly
- minimal user confusion during key match events

## Dependencies

- Feature 04 gameplay state and scoring
- Feature 02 presentation motion
- camera behavior from Feature 05

## References

- [files/game_flow_and_camera_design.md](/home/akash/Desktop/CRICKET/files/game_flow_and_camera_design.md)
- [files/scene_by_scene_setup.md](/home/akash/Desktop/CRICKET/files/scene_by_scene_setup.md)

## AI Agent Tasks

- define HUD regions and priority rules
- specify replay trigger conditions
- write the results screen data contract
- map match events to presentation overlays

## Tests

- score and over indicators update correctly
- replay can start and stop without corrupting match state
- results screen matches final match outcomes

## Exit Criteria

- the player can follow the live match without confusion
- replay and results tell the story of the match
- HUD does not obscure critical gameplay cues

# Feature 05.3: Tutorial and First-Run Flow

## What This Doc Covers

This doc defines how the game teaches the player during the first sessions.

The first-run experience should introduce the game gently, unlock confidence, and avoid dumping every system on the player at once.

## Scope

Include:

- first-run gating
- step-by-step tutorial flow
- contextual prompts
- practice guidance
- return-player skip behavior

Exclude:

- full match setup logic
- camera design details
- progression economy
- networking

## Implementation Tasks

1. Define the tutorial order from movement to batting, bowling, and match flow.
2. Keep each tutorial step focused on one concept at a time.
3. Use contextual prompts that appear only when needed.
4. Allow experienced players to skip or compress the tutorial.
5. Record tutorial completion so the flow is not repeated unnecessarily.

## Expected Output

- a guided first-time experience
- fewer early drop-offs
- a tutorial that can be extended as features grow

## Dependencies

- Feature 03.2 tutorial, nets, and setup
- Feature 05.2 input mapping
- onboarding-related save state

## References

- [files/player_onboarding_and_tutorial_flow.md](/home/akash/Desktop/CRICKET/files/player_onboarding_and_tutorial_flow.md)
- [files/development_plan.md](/home/akash/Desktop/CRICKET/files/development_plan.md)

## AI Agent Tasks

- write the tutorial step order
- define skip and resume rules
- document each prompt and its trigger
- connect tutorial completion to saved profile state

## Tests

- new player sees the correct sequence
- completed tutorial does not repeat unexpectedly
- prompts do not conflict with gameplay input

## Exit Criteria

- the game teaches the basics without overload
- first-run progress is persisted
- tutorial behavior is easy to extend

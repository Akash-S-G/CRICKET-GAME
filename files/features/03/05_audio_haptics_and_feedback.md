# Feature 03.5: Audio, Haptics, and Feedback

## Purpose

This doc gives ownership of audio and tactile feedback so those systems are not orphaned.

## Scope

Include:

- SFX
- music
- commentary hooks
- haptics
- hit feedback
- menu feedback
- match event stingers

## Rules

1. Audio feedback must follow game events, not duplicate gameplay logic.
2. Haptics must be optional and user-configurable.
3. Match events should have a small, consistent feedback palette.
4. Audio should support mobile performance tiers.

## Initial Event Coverage

- match start
- bat contact
- wicket
- boundary
- six
- over complete
- result screen
- menu confirm
- menu back

## Exit Criteria

- audio and haptics have one owner
- feedback is linked to cricket events
- mobile settings can disable haptics cleanly


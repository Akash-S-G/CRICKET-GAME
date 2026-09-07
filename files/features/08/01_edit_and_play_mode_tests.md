# Feature 08.1: Edit and Play Mode Tests

## What This Doc Covers

This doc defines the automated Unity test coverage for the project.

It focuses on the tests that prove gameplay rules, data contracts, and scene behaviors are not breaking as the project grows.

## Scope

Include:

- edit-mode tests
- play-mode tests
- pure domain tests
- smoke tests for scenes and startup

Exclude:

- CI orchestration
- release policy
- analytics transport
- multiplayer provider selection

## Implementation Tasks

1. Identify the highest-value rules and flow paths to test first.
2. Keep domain logic testable without the full scene tree.
3. Add play-mode tests for scene startup, HUD behavior, and session transitions.
4. Make tests readable enough for AI agents to extend safely.
5. Keep tests deterministic and small.

## Expected Output

- coverage of the most failure-prone logic
- fast feedback during development
- a reliable test structure for future features

## Dependencies

- Feature 04 rules engine
- Feature 03 scene flow
- feature 01 data contracts

## References

- [files/TDD.md](/home/akash/Desktop/CRICKET/files/TDD.md)
- [files/milestone_checklist.md](/home/akash/Desktop/CRICKET/files/milestone_checklist.md)

## AI Agent Tasks

- list the first tests to write
- define the test layout and naming conventions
- separate domain tests from scene tests
- document how failures should be diagnosed

## Tests

- ball and rules tests pass consistently
- scene smoke tests detect startup regressions
- play-mode tests cover major user paths

## Exit Criteria

- the project has a sustainable automated test structure
- the most important gameplay logic is covered
- tests are useful to both humans and agents

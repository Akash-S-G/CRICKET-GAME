# Feature 04: Core Cricket Gameplay

## 1. What This Feature Is

This feature implements the actual cricket simulation and rules:

- ball physics
- batting timing
- shot selection
- bowling execution
- fielding control handoff
- wickets
- extras
- overs
- innings
- scorekeeping

## 2. How To Implement It

### 2.1 Domain First

- Put the match logic in the domain layer.
- Keep the rules engine deterministic.
- Separate presentation from simulation.

### 2.2 Ball and Bat

- Implement swing, seam, bounce, and contact.
- Make timing affect outcome.
- Make shot direction and shot type meaningful.

### 2.3 Bowling and Fielding

- Implement bowling input and execution quality.
- Implement fielding handoff from AI to human control.
- Make run-outs and catches authoritative in the domain layer.

### 2.4 Rules

- Implement dismissals.
- Implement overs and innings.
- Implement extras.
- Implement match end conditions.

## 3. Expected Output

- A full solo match can complete.
- Cricket rules behave consistently.
- The game feels like cricket, not a toy prototype.

## 4. Dependencies

- Feature 01 data layer
- Feature 02 animation pipeline
- Feature 03 scene pipeline
- `rules_engine_spec.md`
- `controller_handoff_spec.md`
- `TDD.md`

## 5. References

- [rules_engine_spec.md](../rules_engine_spec.md)
- [controller_handoff_spec.md](../controller_handoff_spec.md)
- [TDD.md](../TDD.md)
- [animation_requirements.md](../animation_requirements.md)
- [development_plan.md](../development_plan.md)

## 6. Exit Criteria

- A match can start, play, and finish.
- The rules are consistent.
- Fielding, batting, and bowling all work together.

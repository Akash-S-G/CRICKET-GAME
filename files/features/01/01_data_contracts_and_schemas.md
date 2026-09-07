# Feature 01.1: Data Contracts and Schemas

## What This Doc Covers

This doc defines the authoritative data shapes for the project. It is the source of truth for game modes, players, grounds, pitches, cameras, delivery tuning, and progression-related configuration.

The goal is to stop ad hoc data from spreading through gameplay code. Every runtime system should read from these contracts instead of inventing its own shape.

## Scope

Include:

- `ScriptableObject` definitions for content authored in Unity
- JSON schema definitions for data that must be versionable or externally editable
- runtime DTOs and mapping rules
- validation rules for missing fields, invalid ranges, and backward compatibility

Exclude:

- save system behavior
- analytics transport
- remote config fetching
- gameplay logic

## Implementation Tasks

1. Define the base data classes for players, teams, grounds, pitches, modes, and camera presets.
2. Split authoring data from runtime data so Unity assets remain editable while runtime code consumes stable DTOs.
3. Add version fields to every schema that may change over time.
4. Define serialization rules for enums, arrays, nested objects, and optional fields.
5. Create a validation layer that reports missing keys, unsupported values, and broken references.
6. Add fallback behavior for partially missing content so the game can still boot in a degraded state.

## Expected Output

- A documented data model with stable field names
- JSON schema files that match the gameplay needs
- Unity authoring assets that map cleanly to runtime structures
- Validation helpers that fail fast in dev builds and degrade safely in production

## Dependencies

- Feature 0 foundation and tooling
- Unity serialization conventions
- agreed project naming rules

## References

- [files/README.md](/home/akash/Desktop/CRICKET/README.md)
- [files/system_design.md](/home/akash/Desktop/CRICKET/files/system_design.md)
- [files/TECH_STACK.md](/home/akash/Desktop/CRICKET/files/TECH_STACK.md)
- [files/mode_config_schema.json](/home/akash/Desktop/CRICKET/files/mode_config_schema.json)
- [files/player_schema.json](/home/akash/Desktop/CRICKET/files/player_schema.json)
- [files/ground_schema.json](/home/akash/Desktop/CRICKET/files/ground_schema.json)
- [files/pitch_schema.json](/home/akash/Desktop/CRICKET/files/pitch_schema.json)

## AI Agent Tasks

- generate the data classes and schema comments
- create conversion utilities between Unity assets and runtime DTOs
- add editor validation and runtime validation
- ensure schema examples are included in docs

## Tests

- schema validation passes for valid payloads
- invalid payloads produce readable errors
- missing optional values use safe defaults
- versioned data can be migrated without breaking boot

## Exit Criteria

- All major game entities have documented schemas
- gameplay systems can reference shared contracts without custom data shapes
- validation catches bad data before release

# Feature 01: Data and Service Layer

## 1. What This Feature Is

This feature builds the reusable data and service backbone for the game.

It includes:

- mode configs
- player profiles
- pitch data
- ground data
- camera config data
- progression data
- save/load
- remote config
- analytics
- session state

## 2. How To Implement It

### 2.1 Data Models

- Define ScriptableObjects for authored game data.
- Mirror important values in JSON schemas.
- Keep balance values in data, not hard-coded in scripts.

### 2.2 Services

- Implement a save service.
- Implement a profile service.
- Implement a session controller.
- Implement remote config loading.
- Implement analytics hooks.
- Implement settings persistence.

### 2.3 Data Flow

- Boot loads config.
- Profile loads settings.
- Session controller creates the active game session.
- Save service stores progression and player state.
- Remote config updates tuning values.

## 3. Expected Output

- The game can load and save player state.
- Modes and tuning values are data-driven.
- Services are reusable across scenes.
- Offline and online paths can share the same data model.

## 4. Dependencies

- Feature 00 foundation
- `system_design.md`
- `TDD.md`
- data schemas in `files/`

## 5. References

- [system_design.md](../system_design.md)
- [TECH_STACK.md](../TECH_STACK.md)
- [mode_config_schema.json](../mode_config_schema.json)
- [player_schema.json](../player_schema.json)
- [ground_schema.json](../ground_schema.json)
- [pitch_schema.json](../pitch_schema.json)
- [delivery_physics_constants.json](../delivery_physics_constants.json)

## 6. Exit Criteria

- Save and load work.
- Data loads from authored sources.
- Services are stable and reusable.
- The project can support later gameplay systems without rework.

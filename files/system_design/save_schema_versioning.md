# Save Schema Versioning

## Purpose

This doc defines how profile and session data are stored, versioned, migrated, backed up, and recovered.

It must be deterministic because save corruption, device changes, and future app updates are guaranteed to happen.

## Storage Rules

Use a file-based save path for primary local persistence.

Do not use `PlayerPrefs` for primary profile data.

Recommended storage layout:

```text
/Application.persistentDataPath/
  profiles/
    <profileId>/
      profile.json
      profile.backup.json
      profile.meta.json
  sessions/
    <sessionId>/
      session.json
      session.backup.json
```

## File Format

- Primary format: JSON
- Encoding: UTF-8
- Compression: optional, only if needed later
- Encryption: optional for sensitive future fields, not required for basic offline profile storage

## Versioning Rules

Every persisted object must include:

- `schemaVersion`
- `dataVersion`
- `updatedAtUtc`
- `sourceAppVersion`

Version handling:

1. Read the file version first.
2. If version matches current schema, load directly.
3. If version is older, run a deterministic migration step.
4. If migration fails, move the bad file to quarantine and restore from backup.
5. If both primary and backup fail, create a fresh profile with safe defaults.

## Migration Rules

Each migration must be:

- pure,
- repeatable,
- documented,
- covered by a test.

Migration functions should follow a simple pattern:

```csharp
ProfileDataV3 Migrate(ProfileDataV2 oldData);
```

Do not mutate the source object in place.

## Conflict Resolution

When local and server states disagree:

1. Use the latest trusted authoritative source for the field in question.
2. Use field-level merge rules instead of overwriting the entire profile.
3. Prefer server state for progression unlocks, currency, and competitive state.
4. Prefer local state for transient settings if they are newer and not conflict-sensitive.
5. Record the conflict for telemetry and debugging.

## Backup Rules

- Every write should produce a new temp file first.
- Replace the primary file only after the temp file is fully written and validated.
- Keep one backup of the previous good save.
- Never leave a partially written save as the active file.

## Required Data Keys

At minimum, persistence should cover:

- profile id
- display name
- guest/platform identity state
- progression state
- unlocks
- settings
- tutorial completion
- selected control presets
- selected camera presets
- last known session summary

## Dependencies

- Feature 01 profile and save service
- feature 01 data contracts
- auth identity flow

## Tests

- load/save round trip
- version migration
- corrupted file recovery
- backup restoration
- field-level merge behavior

## Exit Criteria

- save data can survive app restarts and version changes
- corruption recovery is predictable
- every persisted field has a documented owner


# Feature 01.0: Profile Field List and Versioning

## Purpose

This doc defines the exact fields the profile system must carry.

## Profile Fields

Required fields:

- `profileId`
- `displayName`
- `avatarId`
- `identityProvider`
- `isGuest`
- `tutorialCompleted`
- `selectedModeId`
- `selectedCameraModeId`
- `selectedControlPresetId`
- `progressionLevel`
- `experiencePoints`
- `currencySoft`
- `unlockIds`
- `cosmeticIds`
- `settings`
- `lastSessionSummary`
- `schemaVersion`
- `updatedAtUtc`

## Settings Fields

Required settings:

- `audioMasterVolume`
- `musicVolume`
- `sfxVolume`
- `hapticsEnabled`
- `cameraSensitivity`
- `battingTimingAssist`
- `graphicsQualityTier`
- `languageCode`

## Versioning Rules

1. Every profile payload must include a schema version.
2. Every migration must be monotonic.
3. New fields must provide safe defaults.
4. Removed fields must remain readable through at least one compatibility migration.

## Ownership Rules

- profile service owns identity and persistence,
- progression system owns reward updates,
- tutorial flow owns completion flags,
- settings UI owns user-facing preference changes.

## Exit Criteria

- the profile model is complete enough for implementation
- every field has a clear owner
- migration and fallback paths are defined


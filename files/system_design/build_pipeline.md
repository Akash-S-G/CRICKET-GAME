# Build Pipeline

## Purpose

This doc locks the build and CI tooling so AI-generated changes can be validated continuously.

## Tool Choice

Use:

- GitHub Actions for CI
- GameCI for Unity build automation
- Git LFS for large binary assets

If a later pipeline move is required, document that as a separate decision. Do not leave the tool unspecified.

## Required CI Checks

1. Restore packages.
2. Verify Unity project opens.
3. Run edit-mode tests.
4. Run play-mode smoke tests.
5. Build the target platform.
6. Validate asset references.
7. Publish build artifacts or logs.

## Branch Rules

- Feature branches merge only after the CI checks pass.
- Main branch must always remain buildable.
- Release branches should be cut only from a known-good CI state.

## Versioning Rules

- Version bump should happen at milestone boundaries only.
- Tag format should remain human-readable.
- Store version metadata in a single source of truth.

## Git LFS Rules

Track large assets such as:

- textures,
- meshes,
- audio,
- animation clips,
- cinematics,
- binary scene files if needed.

## Build Artifacts

Each CI run should preserve:

- build logs,
- test logs,
- package versions,
- Unity version,
- commit hash,
- tag or branch name.

## Failure Policy

1. Fail fast on compile or package errors.
2. Mark test failures clearly.
3. Keep the last known good artifact available.
4. Do not auto-merge on partial success.

## Dependencies

- project initialization
- test docs
- version control conventions

## Tests

- CI can run without manual editor setup
- build artifacts are reproducible
- LFS assets are resolved correctly

## Exit Criteria

- one documented build path exists
- CI can validate the project automatically
- release failures are easy to diagnose


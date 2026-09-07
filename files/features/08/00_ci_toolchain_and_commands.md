# Feature 08.0: CI Toolchain and Commands

## Purpose

This document locks the build and test automation toolchain so AI agents do not invent incompatible CI providers or command lines.

## Toolchain Decision

- CI provider: GitHub Actions.
- Unity runner: GameCI Unity Builder and Test Runner containers.
- Unity version: the version pinned by `ProjectSettings/ProjectVersion.txt` and Feature 00.
- Large assets: Git LFS, with checkout enabled in CI.
- Test framework: Unity Test Framework, EditMode and PlayMode assemblies.
- Android build: Development build on pull requests; release-signed build only on protected tags.

Unity Cloud Build may be added later for distribution, but it is not the source of truth for merge validation.

## Required CI Jobs

| Job | Trigger | Required to merge |
|---|---|---|
| `docs-and-schema` | pull request | yes |
| `unity-editmode` | pull request | yes |
| `unity-playmode` | pull request to main | yes |
| `android-development-build` | pull request to main | yes |
| `mobile-performance-smoke` | main/nightly | no, blocks release |
| `release-candidate` | protected tag | yes for release |

## Canonical Commands

Agents must expose equivalent scripts in the repository so local and CI execution use the same entry points:

```text
./scripts/validate-docs.sh
./scripts/validate-schemas.sh
./scripts/unity-tests.sh editmode
./scripts/unity-tests.sh playmode
./scripts/build-android.sh development
./scripts/build-android.sh release
```

The Unity test script must pass `-batchmode -nographics -quit -runTests -testPlatform <EditMode|PlayMode> -testResults <path>` and return the Unity exit code.

## Pull Request Gates

- No compiler errors or test failures.
- All referenced scenes exist and are included in build settings.
- JSON schemas and data assets validate.
- No large binary asset bypasses Git LFS.
- Development Android build completes.
- Build version and commit SHA are embedded in the player diagnostics.

## Caching and Artifacts

Cache the Unity `Library` directory using the pinned Unity version and package lock hash. Upload test XML, logs, build size report, and validation summaries for every required job.

## Acceptance Tests

- A clean runner can build without a developer machine cache.
- A deliberately broken schema fails `docs-and-schema`.
- A deliberately broken scene reference fails build validation.
- A test failure causes a non-zero CI status.
- Artifacts identify commit SHA, Unity version, package lock hash, and build target.

## Dependencies

- `features/00/01_project_manifest_and_tooling_lock.md`.
- `system_design/build_pipeline.md`.
- `features/08/01_edit_and_play_mode_tests.md`.

## Expected Outputs

- Workflow files under `.github/workflows/`.
- Reusable scripts under `scripts/`.
- Test and build artifacts retained for at least 14 days.


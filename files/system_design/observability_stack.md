# Observability Stack

## Purpose

This doc defines the logging, crash reporting, and operational visibility needed to maintain the game after release.

## Required Signals

- startup success/failure
- save load success/failure
- remote config fetch success/failure
- match start/end
- disconnect/reconnect
- major gameplay errors
- CI build and test status

## Logging Rules

1. Use structured logs where practical.
2. Keep logs compact.
3. Include session id, profile id, and build hash when relevant.
4. Avoid sensitive data in logs.

## Crash / Error Reporting

Use one crash reporting provider consistently.

Must support:

- stack traces,
- breadcrumb events,
- build version tagging,
- sampling or rate limiting,
- offline buffering if possible.

## Metrics

Track at minimum:

- boot time,
- scene load time,
- save failures,
- config fetch failures,
- match abandon rate,
- reconnect success rate,
- frame budget violations.

## Sampling

- high-volume events may be sampled,
- failures and crashes should not be sampled away,
- debug builds may log more aggressively than production.

## Dashboard Needs

The team should be able to inspect:

- startup health,
- session health,
- match failure trends,
- performance regressions,
- save and config reliability.

## Dependencies

- build pipeline
- analytics system
- release hardening

## Tests

- logs appear with expected fields
- crash reporting receives a sample error
- metrics are emitted at the right points

## Exit Criteria

- the project can be observed after release
- critical failures are visible
- operational debugging is possible without guesswork


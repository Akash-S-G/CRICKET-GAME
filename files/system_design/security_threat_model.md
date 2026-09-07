# Security Threat Model

## Purpose

This doc lists the main client and service risks that must be assumed from the start.

## Threats

### 1. Client Memory Cheats

Risk:

- local memory manipulation,
- value injection,
- save tampering.

Mitigation:

- validate critical progression server-side when online,
- keep sensitive authoritative state off the client where possible,
- store checksums or signatures for important save payloads.

### 2. Packet Tampering

Risk:

- manipulated network messages,
- replayed session actions,
- forged handoff or score events.

Mitigation:

- server authoritative state,
- message validation,
- nonce or sequence tracking,
- discard out-of-order critical events.

### 3. Save Editing

Risk:

- user edits local files to gain progression,
- invalid schema injection.

Mitigation:

- schema validation,
- checksum,
- migration validation,
- quarantine invalid files.

### 4. Identity Abuse

Risk:

- guest profile collision,
- stale auth tokens,
- profile merging mistakes.

Mitigation:

- explicit auth state,
- token refresh rules,
- profile ownership checks.

## Security Rules

1. Do not trust the client for competitive match outcomes.
2. Log security-relevant failures.
3. Fail safe, not open, for critical progression and authority paths.
4. Keep secrets out of client code where possible.

## Dependencies

- auth service
- persistence layer
- multiplayer authority

## Tests

- invalid save files are rejected
- spoofed state transitions are blocked
- identity merge behavior is deterministic

## Exit Criteria

- the main cheat and tamper paths are documented
- mitigation exists for each major threat
- the architecture does not assume a trusted client


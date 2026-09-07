# UI Architecture and Navigation

## Purpose

This doc defines how the UI is structured so menu, match, and overlay screens do not fight each other.

## Runtime UI Choice

- Use uGUI for runtime UI.
- Use UI Toolkit only for editor tooling if needed.

## UI Structure

### 1. Persistent UIRoot

One persistent root object should hold:

- global navigation,
- modal stack,
- toast messages,
- loading overlays,
- accessibility helpers.

### 2. Screen Stack

Use a stack model:

1. push a screen,
2. show it,
3. pop it when done,
4. keep modal overlays separate from full screens.

### 3. Overlay Layers

Layers:

- full-screen routes,
- modal dialogs,
- HUD overlays,
- transient notifications.

## Navigation Rules

1. Menus and match screens should not share hidden state.
2. Back navigation must be consistent across mobile screens.
3. The home screen should always be recoverable from failure states.
4. Match HUD should never rely on scene-local singleton assumptions.

## Scene Ownership

- Boot scene owns startup validation.
- Home scene owns high-level navigation.
- Match scene owns gameplay HUD and live overlays.
- Replay and results scenes own post-match presentation.

## UI Data Flow

UI should consume:

- view models,
- event payloads,
- session state,
- profile state.

UI should not mutate rules state directly.

## Dependencies

- scene flow docs
- state machine doc
- save/session services

## Tests

- back navigation works consistently
- HUD and modal layers do not conflict
- persistent root survives scene loads

## Exit Criteria

- the navigation model is explicit
- runtime UI is not ambiguous
- scene transitions do not break overlays


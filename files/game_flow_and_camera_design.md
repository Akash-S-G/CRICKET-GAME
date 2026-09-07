# Game Flow, Presentation, and Camera Design

This document explains how the game should start, what the player sees, how selection works, how progression feels, how core features should behave, and how the camera system should be structured.

## 1. Product Goal

The game should feel like a cricket experience that is easy to enter, fast to understand, and deep enough to keep players returning. The first session should be short and clear. Later sessions should reveal more control, more strategy, and more customization.

The design should avoid feeling like a generic sports game menu with cricket attached. It should feel like a cricket product from the moment it opens.

## 2. First Launch Experience

### 2.1 Boot Flow

1. Studio logo and game identity splash.
2. Short loading screen with a live cricket visual, not a static black screen.
3. Player lands on a welcome screen with one clear primary action.
4. The game asks whether the player wants to jump into a quick match, practice, or setup.
5. On first launch, the game offers a guided tutorial before the full menu stack.

### 2.2 First-Look Tone

The game should look:

- fast,
- sporty,
- polished,
- mobile-friendly,
- cricket-specific,
- visually confident without being cluttered.

The user should immediately understand that this is not an esports shooter or a deep management sim. It is a cricket game with strong play-first energy.

### 2.3 First-Run Unique Hook

The first-run flow should highlight what makes this game different:

- AI backfill means a match can start even without a full lobby.
- Touch controls work for mobile first, not as a reduced port.
- Nets mode is a real training tool, not just a menu item.
- Camera modes change how the game feels, so the player can choose a view that matches their style.

## 3. Main Menu Structure

### 3.1 Primary Menu Items

The main menu should stay simple:

- Quick Match
- Nets
- Career
- Tournament
- Online Match
- Customize
- Practice
- Settings
- Profile

### 3.2 Menu Behavior

The menu should prioritize the most likely actions:

- returning players should get one-tap access to the last played mode,
- new players should see a tutorial prompt,
- mobile users should never need to dig through layers to start batting.

### 3.3 Unique Menu Presentation

The menu should feel alive:

- subtle animated stadium background,
- changing crowd noise depending on mode,
- match cards that show current progression,
- player silhouette or squad banner for the active profile,
- clean typography and minimal friction.

## 4. Player Selection and Identity

### 4.1 Profile Selection

The game should support a local player profile system.

Each profile should store:

- player name,
- preferred camera,
- control sensitivity,
- tutorial status,
- progression level,
- unlocked cosmetics,
- favorite mode,
- last played team or roster.

### 4.2 Player Identity Options

Players should be able to create or choose a profile with:

- name,
- avatar or badge,
- favorite handedness,
- preferred batting style,
- preferred bowling style,
- preferred camera.

### 4.3 Squad and Role Selection

Before a match, the player should choose:

- team or squad,
- batting order,
- bowling priorities,
- captaincy mode,
- fielding assist level,
- match format,
- camera mode,
- difficulty preset.

### 4.4 Unique Player Selection Design

Selection should be fast and cricket-like:

- batting order should matter,
- the player should feel the difference between a power hitter, anchor, all-rounder, and specialist bowler,
- the game should preview role impact before the match starts,
- short tooltips should explain what each selection changes.

## 5. Match Start Flow

### 5.1 From Menu to Ball One

A match should start in this order:

1. Mode selection.
2. Team or squad selection.
3. Player role selection.
4. Camera selection.
5. Difficulty and assist selection.
6. Stadium and format selection if relevant.
7. Toss or auto-toss.
8. Short pre-match presentation.
9. First over begins.

### 5.2 Pre-Match Presentation

The pre-match sequence should be short but stylish:

- team banners,
- pitch/stadium visuals,
- captain animations,
- quick summary of format and target,
- a final prompt showing controls.

The goal is to make the match feel like it has started, not like the player is waiting in a settings screen.

### 5.3 Mobile Match Start

On mobile, the player should be able to skip directly into play with minimal setup:

- use last settings by default,
- let the player adjust one or two settings only,
- keep advanced setup tucked behind an optional expand button.

## 6. Progression Design

### 6.1 Progression Goals

Progression should reward time played without breaking balance.

It should make the player feel:

- improvement,
- identity,
- unlocks,
- ownership,
- short-term goals,
- long-term goals.

### 6.2 What Progression Unlocks

Progression can unlock:

- cosmetics,
- stadium themes,
- profile badges,
- camera skins or overlays,
- new practice drills,
- new challenge sets,
- squad customization items,
- special match events,
- career milestones.

### 6.3 What Progression Must Not Break

Progression must not make Main Mode unfair.

It should not turn into pay-to-win or grind-to-win behavior in competitive play.

### 6.4 Unique Progression Style

Progression should be cricket-shaped, not generic:

- batting milestones for strike rate, innings, and shot variety,
- bowling milestones for maidens, wickets, dot-ball pressure,
- fielding milestones for catches, run-outs, and direct hits,
- team milestones for wins, chases, and defended totals,
- style milestones for patience, aggression, or all-round play.

## 7. Core Feature Behavior

### 7.1 Batting

Batting should feel like a timing and intent game.

The player should:

- choose shot intent late,
- see footwork matter,
- understand timing success or failure,
- feel different outcomes for defense, drive, loft, sweep, and aggressive shots.

### 7.2 Bowling

Bowling should feel like control of line, length, and deception.

The player should:

- pick delivery type,
- choose pace or spin variation,
- release with timing,
- see the ball behave according to execution quality.

### 7.3 Fielding

Fielding should feel tactical and responsive.

The player should:

- see AI shape the field,
- take control when the ball enters range,
- feel the handoff as smooth,
- be able to dive, throw, catch, or cut off runs.

### 7.4 AI Backfill

AI backfill should make the game resilient, not robotic.

If a lobby is short, the match should still start. The AI should fill gaps in a way that preserves flow and keeps the game moving.

### 7.5 Unique Feature Philosophy

Every major system should answer one question:

"How does this make cricket feel more like cricket?"

If a feature does not help with timing, pressure, strategy, atmosphere, or cricket identity, it should not be prioritized ahead of those things.

## 8. Camera Design

### 8.1 Camera Goals

The camera system should support different play styles:

- mobile touch readability,
- batting precision,
- bowling control,
- fielding awareness,
- cinematic presentation,
- accessibility.

### 8.2 Camera Modes

#### FPP, Batter View

First-person or near-batter view focused on the striker.

Use when:

- the player wants maximum immersion,
- batting timing is the priority,
- the match is in Nets or a training mode,
- mobile players want a close, readable strike view.

Benefits:

- strongest sense of timing,
- clear contact point visibility,
- immersive batting pressure.

Risks:

- reduced field awareness,
- harder for new players until they learn the game.

#### Batting Broadcast Camera

A classic broadcast-style batting camera positioned behind or beside the batter.

Use when:

- the player wants balanced field view,
- the game is in Main Mode or competitive play,
- readability matters more than immersion.

Benefits:

- easiest for most players to read,
- good shot direction awareness,
- familiar cricket presentation.

#### Bowling End Camera

Camera placed at the bowler’s end, aimed at the batter.

Use when:

- bowling,
- setting up line and length,
- learning delivery timing.

Benefits:

- strong delivery alignment,
- useful for release and target feedback,
- good for mobile precision.

#### Mid-Wicket Tactical Camera

A wider tactical side camera.

Use when:

- field placement matters,
- the player needs to read gaps,
- the game is in a more strategic mode.

Benefits:

- strong sense of space,
- useful for understanding field placements,
- good for slower formats.

#### Chase Camera

A follow-style camera that tracks the ball or active player during fielding.

Use when:

- running between wickets,
- diving saves,
- boundary chases,
- high-intensity fielding moments.

Benefits:

- cinematic energy,
- helps the player feel momentum.

#### Top-Down Tactical Camera

A higher camera for field planning and simplified casual modes.

Use when:

- the player is setting fields,
- the game needs better spatial awareness,
- the mode is casual or mobile-friendly.

Benefits:

- best for tactical overview,
- good for younger or newer players,
- helpful in MinBoundary and Gully modes.

#### Replay Camera

A slow-motion presentation camera used only for highlights, wickets, boundaries, and close calls.

Use when:

- the game wants to celebrate a moment,
- a wicket or big shot just happened,
- the player wants a cinematic recap.

### 8.3 Camera Switching Rules

- The player should be able to lock a camera per mode.
- The game should suggest a default camera based on mode and skill level.
- Camera switching should be simple and fast.
- On mobile, camera changes must never interrupt play flow.

### 8.4 Recommended Default Cameras

- Nets: FPP Batter View or Batting Broadcast Camera.
- Main Mode batting: Batting Broadcast Camera.
- Main Mode bowling: Bowling End Camera.
- Gully: Top-Down Tactical Camera or Batting Broadcast Camera.
- MinBoundary: Chase Camera or Top-Down Tactical Camera.
- Career: Broadcast Camera with occasional replay cuts.

## 9. Visual Style

The game should look distinct from current cricket titles by mixing:

- clean mobile readability,
- subtle broadcast polish,
- animated stadium atmosphere,
- expressive camera work,
- simple but confident UI.

It should not feel overdesigned. It should feel sharp, sporty, and instantly readable.

## 10. Mode Feel Summary

### Quick Match

- Fastest path to play.
- Best for casual sessions.
- Simple setup, minimal friction.

### Nets

- Learning and tuning mode.
- Best for batting, bowling, and camera practice.
- Core mobile onboarding environment.

### Career

- Long-term progression mode.
- Best for player identity and growth.

### Online Match

- Competitive mode.
- Best with broadcast camera and clear assists.

### Gully and MinBoundary

- Casual modes with more flexible camera and simplified control options.
- Best for mobile short sessions.

## 11. Final Design Principle

The game should always answer these questions clearly:

- What am I doing right now?
- Why did that shot or delivery work or fail?
- What do I unlock next?
- What should I play next?
- What camera helps me play best?

If the player can answer those questions at a glance, the design is working.

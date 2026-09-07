# Player Onboarding and Tutorial Flow

This document defines how a new player learns the game from first launch to first successful match. The goal is to remove friction without making the experience shallow.

## 1. Onboarding Goals

The onboarding flow should achieve four things:

- Teach the player how to start a match.
- Teach the player how batting, bowling, and fielding work.
- Show the player how progression and camera modes fit into the game.
- Get the player to a real, playable cricket moment as quickly as possible.

The tutorial should not feel like a classroom. It should feel like a short guided cricket session.

## 2. First-Time Player Journey

### Step 1: Welcome

When the game opens for the first time, the player should see:

- the game identity,
- one clear primary action,
- a short statement of what makes the game different,
- a visible option to jump into a tutorial.

Primary actions:

- `Start Tutorial`
- `Quick Match`
- `Practice`

The tutorial should be the recommended first action, but not a hard lock.

### Step 2: Choose a Style

Before the tutorial starts, the player chooses:

- mobile or controller style,
- preferred camera,
- simple or advanced assist level.

This is where the game begins to feel personal.

### Step 3: Learn the Core Loop

The tutorial should teach the core loop in the same order the player will experience it in a real match:

1. Batting
2. Bowling
3. Fielding
4. Match setup
5. Progression and unlocks
6. Camera switching

## 3. Tutorial Structure

### 3.1 Batting Tutorial

The batting lesson should include:

- stance and timing,
- shot direction,
- footwork,
- defensive vs attacking intent,
- timing feedback,
- edge or miss feedback.

The player should complete a small set of simple goals:

- defend one ball,
- score a single,
- hit a gap shot,
- attempt one aggressive shot,
- experience one mistimed outcome.

This lets the player learn that timing and intent are both important.

### 3.2 Bowling Tutorial

The bowling lesson should include:

- selecting a delivery type,
- choosing line and length,
- release timing,
- basic pace or spin difference,
- what happens when the release is early or late.

The player should complete goals such as:

- hit the target zone,
- vary length,
- bowl one successful delivery,
- bowl one intentionally difficult ball.

### 3.3 Fielding Tutorial

The fielding lesson should include:

- field placement basics,
- direct control handoff,
- dive and pickup,
- throw and release,
- catching and run-out pressure.

The player should see:

- how AI controls the field before handoff,
- when control switches,
- how to react when the ball comes near,
- how to complete a throw or stop a boundary.

### 3.4 Match Setup Tutorial

The setup lesson should explain:

- match formats,
- team or squad selection,
- batting order,
- bowling priorities,
- assist levels,
- camera choice,
- difficulty choice.

The tutorial should avoid long explanations. It should show one example per setting, then move on.

### 3.5 Progression Tutorial

The progression lesson should show that the game rewards:

- play time,
- skill improvement,
- match performance,
- short sessions,
- long-term goals.

It should explain that unlocks are mostly:

- cosmetics,
- camera options,
- practice content,
- challenge content,
- profile identity items.

It should also make clear that competitive balance is protected.

## 4. Tutorial Flow by Mode

### 4.1 Mobile Flow

Mobile onboarding should be the shortest version:

- touch controls first,
- Nets mode first,
- simplified camera suggestions,
- short practice drills,
- fast access to Quick Match.

The mobile path should not overwhelm the player with menus.

### 4.2 Controller Flow

Controller onboarding can expose more depth:

- dual-stick batting explanation,
- bowling detail,
- fielding handoff behavior,
- camera switching,
- competitive mode setup.

### 4.3 Returning Player Flow

Returning players should not be forced through the full tutorial again.

Instead, they should see:

- a quick resume prompt,
- a short reminder of any updated controls,
- a suggestion for the last played mode,
- a skip path to Quick Match or Nets.

## 5. Practice and Nets Integration

Tutorial content should flow directly into Nets mode.

Nets should be used for:

- batting timing practice,
- bowling target practice,
- camera familiarity,
- control calibration,
- assist testing.

The goal is to make Nets the place where a player becomes confident.

## 6. Feedback Design

The tutorial should use feedback that is immediate and specific.

### Good Feedback

- `Good timing`
- `Late shot`
- `Shot played to cover`
- `Bowled outside off`
- `Field handoff complete`
- `Catch taken`

### Bad Feedback

- vague warnings,
- hidden failure states,
- unclear penalties,
- explanations that are too technical.

Feedback should always answer:

- what happened,
- why it happened,
- what to try next.

## 7. Camera Learning

The tutorial should explicitly teach camera modes.

### Recommended teaching order

1. Batting Broadcast Camera
2. Bowling End Camera
3. FPP / Batter View
4. Top-Down Tactical Camera
5. Chase Camera

The player should learn:

- when each camera is best,
- how camera choice changes readability,
- how to lock a preferred camera for their play style.

## 8. Tutorial Completion State

When the tutorial is complete, the player should have:

- started at least one batting sequence,
- bowled at least one set of deliveries,
- performed at least one fielding action,
- selected a camera,
- seen progression rewards,
- reached a real match or Nets session without confusion.

The tutorial is successful if the player can now start a match without help.

## 9. Design Principles

- Keep the flow short.
- Teach one thing at a time.
- Show the player how the game feels before explaining everything.
- Let the player play quickly.
- Make each lesson directly relevant to a real match.

If the tutorial feels long, the design has failed.

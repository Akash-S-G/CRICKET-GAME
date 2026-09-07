# Competitive Analysis

This document compares the current design against the cricket games I fetched from the web. It is a market-position and feature-gap note, not a code review.

## 1. Current Market References

### 1.1 Mobile Market References

The most relevant mobile competitors are:

- `Real Cricket 24`
- `World Cricket Championship 3`
- `Stick Cricket Super League`
- `World Cricket Championship 2`-style mobile cricket progression systems

These games currently emphasize:

- touch-first batting and bowling,
- big shot libraries,
- commentary,
- motion-captured fielding or batting,
- tournaments and live events,
- career or progression loops,
- licensed or semi-licensed presentation,
- fast reward loops that work in short sessions.

### 1.2 PC/Console Reference Set

The strongest current reference points are:

- `Cricket 26`
- `Cricket 24`
- `Cricket 22`
- `Cricket 19`
- `Ashes Cricket`
- `Don Bradman Cricket 17`
- `Don Bradman Cricket 14`
- `Cricket 07`
- `Cricket 2005`
- `Brian Lara International Cricket 2005`
- `Brian Lara International Cricket 2007`
- `Cricket Captain 2024`

The recent Big Ant games lead on:

- licenses,
- presentation,
- commentary,
- career structure,
- online play,
- created content,
- modern visuals.

The older EA and Codemasters-era games often lead on:

- broader mode variety for their era,
- strong simulation feel,
- memorable batting systems,
- pace and simplicity.

## 2. Where This Project Already Looks Strong

Based on the docs in this repo, the project already has clear strengths:

- AI backfill means the game can stay playable when lobbies are incomplete.
- The same core simulation is meant to power Main Mode, Gully, MinBoundary, and Nets.
- The rules engine is explicitly state-machine driven.
- LBW and run-out logic are designed to be deterministic and testable.
- Fielding control handoff is treated as a first-class network problem.
- The architecture is data-driven rather than hard-coded around one mode.

That is already better structured than many existing cricket games from an engineering perspective.

## 3. Main Gaps Versus Current Mobile Games

### Touch UX and onboarding

Mobile cricket leaders are already very strong at making the first minute playable. They provide clear touch controls, short onboarding, and gameplay that works in portrait or landscape-friendly phone sessions.

This project should add:

- a first-run tutorial that teaches batting, bowling, and fielding in under a minute each,
- optional practice drills for timing, release, and fielding,
- touch sensitivity and gesture calibration,
- very obvious input feedback for missed timing or bad line/length.

### Session structure

Current mobile titles are built around short, repeatable loops. This project should explicitly support:

- quick match,
- nets/practice,
- short tournament modes,
- save/resume for interrupted sessions,
- daily mission loops,
- low-commitment play when the user only has 2-5 minutes.

### Retention systems

The market leaders keep players returning with:

- events,
- challenges,
- career mode,
- rewards,
- leagues or divisions,
- upgrade paths,
- team building,
- badges or progression milestones.

This project currently has some progression support, but it should add a more explicit mobile retention layer.

### Content breadth

The mobile leaders already offer combinations of:

- wide batting shot libraries,
- commentary,
- tournaments,
- dynamic AI,
- career progression,
- custom squads,
- licensed players or teams.

This design is still narrower than the mobile market unless it adds a stronger progression and content layer.

### Design Requirements to Close the Gap

To compete on mobile, the design should explicitly include:

- A first-run tutorial that teaches batting, bowling, and fielding with very low friction.
- Quick Match and Nets as the two default modes for short sessions.
- Offline play as a first-class experience, not an afterthought.
- Save/resume so interrupted phone sessions do not feel wasted.
- Daily missions, seasonal events, or challenge ladders to create return reasons.
- A progression system that works in tiny play sessions.
- Assist options that make touch controls feel fair on smaller screens.
- A UI that is readable one-handed or on smaller devices.
- Performance tiers that protect low-end phones.
- Mobile-friendly commentary and feedback that do not depend on broadcast-style presentation.

### Visual and performance expectations

On mobile, visuals have to be good enough without becoming heavy. The important comparison is not just raw fidelity, but whether the game feels polished on a phone screen.

This game currently needs:

- aggressive mobile optimization,
- UI designed for thumbs and smaller screens,
- readable score, field, and shot feedback,
- lower battery/performance impact than a console-style sim.

## 4. Recommended Improvements

### Priority 1: Mobile-first onboarding

- Add a proper touch tutorial that teaches batting, bowling, and fielding with examples.
- Add gesture calibration and sensitivity settings.
- Add clear fail feedback so the player understands timing, line, and length mistakes immediately.
- Add practice drills that can be finished in under a minute.

### Priority 2: Short-session retention

- Add quick match, nets, and challenge modes for 2-5 minute sessions.
- Add daily missions and reward loops.
- Add save/resume for interrupted mobile play.
- Add short tournament or event ladders that can be completed in small bursts.

### Priority 3: Fielding and controls

- Keep touch controls simple enough to work on a small screen.
- Improve one-handed or low-complexity assist options.
- Make shot presets and batting direction more legible.
- Keep control handoff polished, especially on slower devices.

### Priority 4: Content depth

- Add more ground types and pitch types.
- Expand beyond one flagship format by using `ModeConfig`.
- Add women’s cricket and more domestic structure if licensing allows.
- Add scenario or challenge modes once the core loop is stable.

### Priority 5: Presentation and polish

- Add a scorebug and UI that stay readable on phones.
- Improve commentary and match feedback.
- Add replay/highlight support for big moments.
- Reduce battery drain and loading friction.

### Priority 6: Social play and longevity

- Add richer online competition.
- Add custom content sharing.
- Add career persistence and meaningful progression.
- Add highlights or replay sharing when the data flow is already present.

## 5. Design Tradeoffs

The repo intentionally chooses some things the market leaders do not always prioritize:

- AI backfill over lobby fragility.
- Data-driven modes over mode-specific code.
- Physics fidelity over arcade shortcuts.
- Small, testable rules and handoff systems over broad feature sprawl.

That is a good technical choice, but it means the project must still invest in visual and presentation polish to avoid feeling underfinished compared with commercial cricket titles.

## 6. Bottom Line

If the goal is to beat the current cricket games in feel, the biggest gaps to close are:

1. batting animation and timing feedback,
2. fielding and handoff polish,
3. broadcast-like presentation,
4. broader content depth,
5. higher-fidelity art and stadium assets.

If the goal is to beat them in robustness and engineering clarity, this design is already on a stronger path than most of the market.

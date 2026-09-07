# UI Design System - Premium Rectangular Cricket (No Rounded)

> **For: Mobile-first cricket. Build same project for other machine.** Style: **Sharp rectangles, not rounded. Premium dark + gold. Inspired by EA FC Mobile 26 / eFootball 2024 / COD Mobile / PUBG Mobile / Real Cricket 24.** Aligns `files/system_design/ui_architecture_and_navigation.md:8` uGUI + `files/AI_AGENT_PACKAGES_AND_TOOLS.md:1` §6 MCP.

## 0. Inspiration Board (Why Rectangular Premium)

| Game | What we steal | What we avoid |
|---|---|---|
| **EA FC Mobile 26** `ea.com/games/ea-sports-fc/fc-mobile` `2025-09-25` fresh UI update, `whativylearned 2024-09-11` study | Sharp card grid, dark bg + gold/emerald accent, rectangular player cards with **inner bevel + drop shadow**, daily login as rect boxes `Pixune 2026` | Not FIFA rounded pills |
| **eFootball 2024** `konami.com` | Hard 2px borders, rect CTA `Play Match`, top bar scorebug `48dp` | Not soft gradients |
| **Real Cricket 24** `naughtygames` | Green stadium palette + white text, rect HUD `GDD.md:40` mobile readability | Not cluttered green flood |
| **COD Mobile / PUBG Mobile** `Pixune 2026` | **HUD customizable, competitive readability, flat rect buttons 44dp** `PUBG` `60 FPS` | Not military texture overload |
| **Clash Royale** `Supercell` `Pixune 2026` | Bold hierarchy, rect boxes even on cartoon - proves rect works | Not cartoon palette |

**Premium = Dark canvas + 1 metallic accent + 2px hard edge + 8px top highlight.** `Figma GamePlan EA Design System` `6 users` confirms EA uses same hard rect cards across sports.

## 1. Principles (Rectangular Only)

1. **No rounded.** `border-radius: 0` everywhere. Only `2px` chamfer allowed on icon inside rect. No `ROUND_FULL` `stitch` - use `ROUND_TWO` max `4dp` if Unity forces.
2. **Rect + bevel.** Every card/button: `1px outer stroke #1A1A1A` + `1px inner highlight #FFFFFF 12%` top edge. Drop shadow `y 4 blur 8 20%`.
3. **Dark premium.** BG `#0A0F14` (stadium night) not white. Content `#FFFFFF` 87%. Accent metallic gold `#D4A017` (Real Cricket gold) not bright green.
4. **Grid 4dp.** All layout on 4dp `system_design.md:188` Mobile `44dp` tap `mobile_roadmap.md:24` - rect fits grid, not offset by radius.
5. **Fast.** Transitions `150ms easeOutQuad` `features/03_scene_and_ui_pipeline.md:28` `0.30s` blend is max `animation_and_scene_pipeline_roadmap.md:318` - rect feels snappy.

## 2. Color System

### 2.1 Palette

| Token | Hex | Use | Inspired |
|---|---|---|---|
| `bg_primary` | `#0A0F14` | App bg, Boot | EA FC dark `GamePlan` |
| `bg_card` | `#131A22` | Rect card fill | eFootball dark |
| `bg_card_elevated` | `#1B2430` | Shop premium card | COD Mobile |
| `border` | `#2A3441` | Rect stroke 1px | EA FC |
| `accent_gold` | `#D4A017` | CTA, XP, gold card top bar 8px `Pixune 2026` premium | Real Cricket gold |
| `accent_gold_press` | `#B68B12` | Pressed | Darken 15% |
| `text_primary` | `#FFFFFF` 87% | Heading | All |
| `text_secondary` | `#9AA8B8` | Body | eFootball grey |
| `success` | `#18C77C` | Win, good timing `GDD.md:214` | PUBG green |
| `danger` | `#E54848` | Wicket, miss | EA red |
| `info` | `#3B82F6` | Bowling variation | COD blue |

**No pure green pitch flood.** Pitch green `#1A6A3A` only in `Gameplay` scene, UI stays dark for contrast `game_flow_and_camera_design.md:5`.

### 2.2 Gradients (Subtle Premium)

- **CTA Gold:** `linear 90deg #D4A017 -> #B68B12` 1px top highlight `#FFFFFF 15%`. Not rainbow.
- **Card header:** `Gold 8px top bar` on `bg_card` rect - instant premium `EA FC player card`.
- **HUD:** `bg_card 85%` opacity, not glass.

## 3. Typography

| Style | Font `system_design.md:443` | Size | Weight | Use |
|---|---|---|---|---|
| `Display` | `Montserrat` Bold (head) `TDD.md:5` fallback `Oswald` | 24sp | 700 | Score `50/2` `Prefabs/UI/HUD` |
| `Heading` | `Montserrat SemiBold` | 18sp | 600 | Card title `Quick Match` |
| `Body` | `Inter` `system_design.md:443` | 14sp | 400 | Desc |
| `Label` | `Inter Medium` | 12sp caps `+1.5` tracking | 500 | `OVER 12.3` `mobile_roadmap.md:24` |
| `Mono` | `JetBrains Mono` `1.11.2` | 12sp | 400 | `Delivery 138 kph` `delivery_physics_constants.json:20` |

All caps for labels only. Letter spacing `1.5px` for rect tightness.

## 4. Spacing / Grid / Layout

- **Base:** `4dp` grid `system_design.md:188`.
- **Tap target:** `48x48dp` min `mobile_roadmap.md:24` + `system_design/input_action_maps.md:31` `44dp` - rect button 48h not pill 36h.
- **Margins:** `16dp` outer `files/ASSET_CATALOG.md:1` `HUD 1920x120` top bar aligns 16.
- **Gutter:** Cards `12dp` between rects `12dp = 3*4`.
- **Safe area:** `Window > UIRoot` `system_design/ui_architecture_and_navigation.md:16` respects notch 24dp.

## 5. Components - Rectangular Spec (Implement in uGUI 9-slice, radius 0)

### 5.1 Buttons (Core - All Rect, Not Rounded)

| Variant | Fill | Border | Text | State | Size |
|---|---|---|---|---|---|
| **Primary Gold CTA** | `Gold grad` `D4->B68` | `1px #1A1A1A outer` + `1px #FFF 12% top inner` | `Inter Bold 14 caps` `#0A0F14` | Hover `#FFF 8%` overlay, Press `#B68B12` 95% scale 0.97, Disabled 40% | `H 48 minW 160` `11 scenes` `scene_by_scene_setup.md:14` `Start Match` |
| **Secondary Dark** | `bg_card #131A22` | `1px #2A3441` | `Inter Med 14 caps` `#FFFFFF` | Same | `H 44` `Cancel` |
| **Ghost Text** | Transparent | None `1px #2A3441 bottom underline rect` | `Inter 12 caps` `#9AA8B8` | Underline `#D4A017` | `H 32` |
| **Icon Rect** | `bg_card` square | `1px #2A3441` `2px chamfer` | Icon `24` | Press 0.95 | `48x48` `CameraNext` `system_design/input_action_maps.md:16` |

**uGUI:** `Image Type: Sliced` border `4` all zero radius. `Button` `Transition: SpriteSwap` rect sprites. Not `Rounded` prefab.

**Do not use:** Capsule, pill `H 36 radius 18`, `Material You` rounded cards. Replace with rect `48` even for `Daily Reward`.

### 5.2 Cards (Shop, Mode, Player)

- **Structure:** Rect `bg_card #131A22` `border 1px #2A3441` top gold bar `8x full width` `radius 0`. Shadow `4y 12blur 20% #000`. Inner top highlight `1px #FFF 10%`.
- **Mode Card `Home_Menu` `scene_by_scene_setup.md:51`** `320x180` rect: top bar gold + mode image `16:9` cropped hard edge + title `Montserrat 18` bottom + `Play` Primary Gold full-width bottom `48h` rect.
- **Player Card:** `120x160` rect `border 2px #D4A017` on gold tier `files/progression_reward_schema.json:1` - elite feels premium not rounded.
- **Stats Row:** Rect `bg_card_elevated` `H 40` `label #9AA8B8` `12 caps` left, value `Inter Bold 14` right, `1px #2A3441` bottom divider hard line.

**Inspired:** `EA FC Mobile` player cards are hard rect with inner bevel even on mobile portrait - copy exactly.

### 5.3 HUD / Scorebug (Competitive Readability `PUBG` `Pixune 2026`)

- **Scorebug `Prefabs/UI/HUD` `files/ASSET_CATALOG.md:1` `1920x120`:** Rect `bg_card 85%` `1px border` hard edge top `8` padding 12. Elements rect aligned: `Team 48/2` `Overs 4.2` `RR 7.1` `Montserrat 24` no bubble. `Delivery 138kph` mono `JetBrains 12` rect chip `bg_card_elevated 1px stroke`.
- **Controls `Match_Play`:** Rect buttons `64x64` (Larger than `48` for touch) `system_design/input_action_maps.md:31` `40px min` + label below `12 caps` hard rect not floating circle. `PUBG`-like HUD customizable.

### 5.4 Navigation / Overlays (`system_design/ui_architecture_and_navigation.md:16` stack)

- **Tab Bar `Home_Menu`:** Rect `bg_card` `H 56` top `1px #2A3441` underline active `8px #D4A017` rect indicator (not dot). `Home/Nets/Career/Online`.
- **Modal:** Rect `bg_card_elevated` `maxW 360` `1px #2A3441` `shadow 8y` no radius, `X` icon rect `48` top-right.
- **Toast:** Rect `bg_card 95%` `H 36` `12 padding` `success #18C77C` left bar `4px` rect.

## 6. Motion (Rect Snappy)

- **Transitions:** `150ms easeOutQuad` `features/03_scene_and_ui_pipeline.md:28` - rect slide `12dp` + fade, no scale bounce. `Cinemachine 3.0.1` `animation_and_scene_pipeline_roadmap.md:318` `0.20-0.35s` same ease.
- **Button press:** Scale `0.97` `80ms` + `gold press #B68B12`, no radius morph.
- **Card hover (PC):** Elevate `y 2` `100ms` shadow `8->12`, border `#D4A017` 1px.

## 7. Iconography

- **Style:** Outline `2px` stroke `24x24` square grid, sharp corners `2px` chamfer not round. Fill on active `gold`.
- **Set:** `Bat` `Ball` `Stumps` `Trophy` `Settings` `Camera` `system_design/input_action_maps.md:16` `CameraNext` - line weight `2`.

## 8. Mobile Ergonomics (`system_design.md:188` tiers `Pixune 2026`)

- One-hand: Primary CTA bottom `16` margin `48h` thumb reach `features/03_scene_and_ui_pipeline.md:28`.
- Low tier `Adreno 610 1.5M/60` `features/06/05_device_tier_budget.md:1`: Disable shadow `y 2` not `4`, no blur `4`, keep rect border `1px` - perf not style.
- Font scale `100%` not `85%` - `GDD.md:40` readable `#FFF 87%` on `#0A0F14`.

## 9. Unity uGUI Implementation (Agent)

```csharp
// Button prefab rect - not rounded
// Hierarchy: UIRoot/Persistent system_design/ui_architecture_and_navigation.md:16
// Image: Type=Sliced Border=4, Pixels Per Unit 100, Sprite rect 64x64 radius 0 9-slice
// Button: Transition SpriteSwap states Gold grad sprites (normal/pressed)
// Shadow: Shadow component distance (0, -4) blur via material, not separate rounded
// Text: TMP Inter Bold 14 caps tracking 1.5, color #0A0F14 on gold
// Layout: VerticalLayoutGroup padding 16 spacing 12 child rect 48h
```

**9-slice rect:** Export `rect_button_gold_64.png` `64x64` border `4` all sides solid, no corner curve. Import `Sprite Mode: Single` `MeshType: FullRect`.

## 10. Screen Layouts (Hard Rect Wire, 11 scenes `scene_by_scene_setup.md:14`)

- **Boot:** Center rect `bg_card` `360x200` `gold bar 8` + `Loading 0..1` rect progress bar `H 8` `bg #2A3441` fill `gold` radius 0 `IContentService` `system_design/service_interfaces.md:16`.
- **Home_Menu:** Top `UIRoot` persistent, `ModeCards` horizontal scroll rect `320x180` `12 gutter`, `DailyReward` rect `H 80` gold left bar, bottom `Tab Bar 56`.
- **Match_Play:** Top `Scorebug 1920x120` rect, center `Camera` `Cinemachine 3.0.1` `48-62 FOV` `animation_and_scene_pipeline_roadmap.md:318`, bottom controls `64x64` rect row `16` margin.
- **Results:** Rect `bg_card_elevated` `360x400` stats rows `H 40` `1px divider`, `Unlock` gold top bar.

## 11. Dos and Don'ts (Hallucination Guard `AGENT.md:1` §4)

| Do (Rect Premium) | Don't (Rounded Cheap) |
|---|---|
| `radius 0` `1px #2A3441` `gold 8px top` `EA FC` | `radius 12-24` pill/capsule `Material 3` `CLASH` rounded |
| `48h` rect CTA `Mont 14 caps` `Pixune PUBG` | `36h` small rounded CTA |
| `1px inner highlight top` bevel | Flat without highlight |
| Dark `#0A0F14` + gold `#D4A017` `Real Cricket` | Green flood or white bg generic |

## 12. Figma + Assets Needed

- **Figma:** Clone `GamePlan EA Design System` `figma.com/community/file/1443366636816575662` `6` users as base - replace radius `12` with `0`, keep gold `#D4A017`.
- **Export:** `rect_card.png` `rect_button_gold.png` `gold_bar_8.png` `icon_24.png` 9-slice to `Assets/_Project/Art/UI/` `unity_ai_workflow_and_project_structure.md:18` (build machine).
- **Validate:** `Window > Cricket > Validate` checks `Source` not needed for UI (UI rect not mocap).

## 13. References

- `files/system_design/ui_architecture_and_navigation.md:16` `uGUI` `Stack`
- `files/FREE_3D_AI_GENERATION_TOOLS.md:1` `UModeler X` not needed for UI rect (code only)
- `AGENT.md:1` §4 `TDD.md:105` `_Project` wins, `GDD.md:40` dark premium wins `TECH_STACK.md:28` PC
- Inspo: `ea.com/games/ea-sports-fc/fc-mobile` `2025-09-25` `whativylearned 2024-09-11` `Figma GamePlan` `dribbble.com/tags/football-ui` `pixune.com 2026` `PUBG/Clash Royale` `Pixune 2026`


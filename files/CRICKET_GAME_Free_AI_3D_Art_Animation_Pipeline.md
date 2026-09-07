# CRICKET-GAME — Free AI 3D Art & Animation Production Pipeline

## 1. Purpose

This document defines a practical production pipeline for creating the 3D models, environments, materials, rigs, animations, and visual assets required for the CRICKET-GAME project using primarily free/open-source software and free AI tiers.

The pipeline is designed for a solo or AI-assisted developer and is intended to integrate with the project's existing Unity 6 architecture and data-driven gameplay design.

The central principle is:

> **AI accelerates asset creation; Blender remains the central source of truth for game-ready assets.**

Recommended high-level pipeline:

```text
References
    ↓
AI concept generation / research
    ↓
AI 3D generation where useful
    ↓
Blender cleanup / modeling / retopology
    ↓
UVs + materials + textures
    ↓
Rigging
    ↓
Motion capture / animation creation
    ↓
Blender animation cleanup
    ↓
IK + animation events
    ↓
FBX export
    ↓
Unity import
    ↓
Animator / Playables / gameplay integration
```

---

# 2. Core Tool Stack

| Area | Primary Tool | Cost | Purpose |
|---|---|---:|---|
| 3D modeling | Blender | Free | Main modeling, cleanup, UVs, rigging, animation |
| 3D asset generation | Tripo / Meshy / equivalent free tiers | Free tier | Rapid object/blockout generation |
| Local AI 3D | Hunyuan3D or equivalent open model | Free/open source | Optional local 3D generation |
| Rigging | Mixamo | Free | Initial humanoid auto-rigging and generic animations |
| Motion capture | Rokoko Video / equivalent | Free tier | Video-to-motion capture |
| Local mocap | MediaPipe-based tools | Free/open source | Alternative motion extraction |
| Image generation | Local Stable Diffusion / ComfyUI or available free services | Free/open source | Concepts and texture references |
| Image editing | GIMP | Free | Texture cleanup and editing |
| Procedural materials | Material Maker | Free | PBR material creation |
| HDRIs / textures | Poly Haven | Free | Environment and PBR assets |
| Game engine | Unity 6 | Free for eligible users | Final game integration |

Official references:

- Blender: https://www.blender.org/
- Mixamo: https://www.mixamo.com/
- Rokoko: https://www.rokoko.com/
- Poly Haven: https://polyhaven.com/
- GIMP: https://www.gimp.org/
- Material Maker: https://www.materialmaker.org/

### Important

AI services frequently change quotas, features, and pricing. Therefore:

- Do not make the game pipeline dependent on one cloud AI service.
- Keep Blender as the permanent production tool.
- Prefer open/local tools when practical.
- Treat cloud AI as an accelerator.

---

# 3. Art Production Philosophy

The game should not be built by generating one unique 3D model for every player, stadium, prop, or animation.

Instead, use reusable systems.

```text
Base Asset
    ↓
Variants
    ↓
Materials / Textures
    ↓
Configuration
    ↓
Unity Runtime Assembly
```

For characters:

```text
One base body
    +
Body variations
    +
Hair
    +
Face
    +
Uniform
    +
Shoes
    +
Equipment
    =
Many players
```

For stadiums:

```text
Modular stand
    +
Seats
    +
Floodlights
    +
Pavilion
    +
Advertising
    +
Pitch
    =
Multiple stadium configurations
```

For animations:

```text
Base animation
    +
Timing
    +
IK
    +
Additive layers
    =
Multiple gameplay outcomes
```

---

# 4. Asset Categories

The CRICKET-GAME art pipeline should be divided into:

```text
ART/
├── Characters
├── Cricket Equipment
├── Stadiums
├── Pitch
├── Props
├── Materials
├── Textures
├── Animations
├── VFX
├── UI
├── Crowd
└── Lighting / Environment
```

---

# 5. Production Order

Do not attempt to create the entire asset library before the first playable build.

Recommended order:

```text
1. Pitch
2. Ball
3. Bat
4. Base player
5. Player rig
6. Idle / walk / run
7. Batting stance
8. Bowling action
9. Three batting shots
10. Basic fielding
11. Catch
12. Throw
13. Simple stadium
14. Camera
15. Animation events
16. IK
17. Unity integration
18. Mobile optimization
19. Expanded asset library
```

The first visual milestone should be:

```text
Bowler
   ↓
Run-up
   ↓
Ball release
   ↓
Ball physics
   ↓
Batsman reacts
   ↓
Shot animation
   ↓
Bat-ball contact
   ↓
Ball travels
   ↓
Fielder reacts
```

---

# 6. Stadium Production

## 6.1 Do Not AI-generate the entire stadium

Large AI-generated environments often produce:

- messy topology
- inconsistent scale
- unnecessary geometry
- poor UVs
- difficult collision setup
- poor mobile performance

Instead, use AI for visual references and Blender for the actual modular environment.

---

## 6.2 Stadium Components

Create:

```text
Stadium/
├── Ground
├── Pitch
├── Boundary
├── Stands
├── Seats
├── Pavilion
├── Dressing Rooms
├── Dugouts
├── Floodlights
├── Scoreboard
├── Advertising Boards
├── Railings
├── Stairways
├── Entrances
└── Props
```

---

## 6.3 Modular Stadium Kit

Create reusable Blender assets:

```text
stadium_kit/
├── stand_small.blend
├── stand_large.blend
├── seats.blend
├── stairs.blend
├── railing.blend
├── floodlight.blend
├── scoreboard.blend
├── advertising_board.blend
├── boundary_wall.blend
├── pavilion.blend
├── dugout.blend
└── camera_platform.blend
```

Construct stadiums from these modules.

Example:

```text
Stadium A
= 4 × large stands
+ pavilion
+ 4 × floodlights
+ scoreboard

Stadium B
= 6 × small stands
+ pavilion
+ 4 × floodlights
+ scoreboard
```

---

# 7. Pitch

The pitch should be modeled manually in Blender.

Components:

```text
Pitch
├── Outfield
├── Pitch strip
├── Batting crease
├── Bowling crease
├── Return crease
├── Stumps
├── Bails
└── Boundary rope
```

Keep the physical dimensions consistent with the game's coordinate system.

Use Unity's physics/gameplay systems for actual ball behavior rather than embedding gameplay logic into the model.

---

# 8. Cricket Ball

The ball should be modeled manually.

Structure:

```text
Ball
├── Leather shell
└── Seam
```

The seam can be created using:

- Blender curves
- procedural geometry
- normal maps

The ball should have:

```text
Base Color
Normal
Roughness
```

Do not AI-generate the ball unless it is being used only as a concept/reference.

---

# 9. Cricket Bat

The bat is another asset that should preferably be modeled manually.

Structure:

```text
Bat
├── Blade
├── Handle
├── Grip
└── Optional branding
```

Create multiple cosmetic variants from the same basic mesh:

```text
bat_01
bat_02
bat_03
bat_04
```

Keep gameplay collision/physics configuration separate from cosmetic appearance.

---

# 10. Cricket Equipment

Useful equipment assets:

```text
Equipment/
├── Bat
├── Ball
├── Helmet
├── Batting Gloves
├── Pads
├── Shoes
├── Wicketkeeping Gloves
├── Cap
├── Sunglasses
├── Stumps
├── Bails
├── Boundary Rope
├── Kit Bag
└── Water Bottle
```

These are good candidates for AI 3D generation followed by Blender cleanup.

Recommended workflow:

```text
AI 3D Generator
    ↓
OBJ / GLB / FBX
    ↓
Blender
    ↓
Scale correction
    ↓
Delete unwanted geometry
    ↓
Retopology
    ↓
UV
    ↓
Materials
    ↓
Export FBX
```

---

# 11. Player Character Strategy

## 11.1 Create one excellent base player

Do not create 20–50 unique player meshes initially.

Create:

```text
Base_Player
```

Requirements:

- humanoid topology
- clean deformation
- proper proportions
- clean UVs
- PBR materials
- compatible humanoid skeleton
- sufficient facial/body detail for the target platform

---

## 11.2 Runtime Variations

Create player diversity using:

```text
Body Type
Skin Material
Face
Hair
Facial Hair
Uniform
Shoes
Helmet
Gloves
Pads
Accessories
```

Conceptually:

```json
{
  "bodyType": 2,
  "skin": 5,
  "hair": 3,
  "uniform": 1,
  "shoes": 4,
  "helmet": true
}
```

Unity can assemble these appearance options.

---

# 12. AI 3D Generation for Characters

AI 3D generation can be used to generate:

- initial character concepts
- generic player bodies
- equipment
- props
- accessories

However, raw AI character meshes often require substantial cleanup.

Common issues:

```text
Non-manifold geometry
Bad topology
Uneven density
Broken fingers
Incorrect proportions
Messy UVs
Overlapping geometry
Texture artifacts
```

Therefore:

> **Never assume an AI-generated character is production-ready.**

Use AI generation for speed, then make the asset game-ready in Blender.

---

# 13. Character Rigging

Recommended initial pipeline:

```text
Character Mesh
    ↓
Mixamo Auto-Rig
    ↓
Humanoid Skeleton
    ↓
Download FBX
    ↓
Blender Cleanup
    ↓
Unity Humanoid Avatar
```

Mixamo is especially useful for:

- idle
- walk
- run
- jump
- fall
- generic reactions

Official tool:

https://www.mixamo.com/

Cricket-specific animations should generally be created separately.

---

# 14. Cricket Animation Categories

The initial animation library should contain:

```text
Animations/
├── Locomotion
├── Batting
├── Bowling
├── Fielding
├── Wicketkeeping
├── Running Between Wickets
├── Reactions
├── Wickets
└── Celebrations
```

---

# 15. Initial Animation Library

## 15.1 Locomotion

Start with:

```text
Idle
Walk
Jog
Sprint
Start Running
Stop Running
Turn Left
Turn Right
Backpedal
Side Step
```

---

## 15.2 Batting

Initial set:

```text
Batting Idle
Batting Ready
Defensive Shot
Straight Drive
Cover Drive
Cut
Pull
Sweep
Lofted Shot
Late Shot
Early Shot
Miss
Edge
Hit Wicket Reaction
```

Do not create every possible shot initially.

Start with approximately 8–12 meaningful shot families.

---

# 16. Batting Animation Architecture

A batting action should be represented as:

```text
Batting Animation
       +
Timing Quality
       +
Shot Direction
       +
Power
       +
Footwork
       =
Final Gameplay Result
```

Timing:

```text
Early
Good
Late
```

The animation itself should not determine the final ball trajectory.

Gameplay should remain authoritative.

---

# 17. Bat Contact Event

Every batting animation should contain a gameplay-relevant contact event:

```text
BatContact
```

Conceptually:

```text
0% ----------------------------- 100%
                     ↑
                  BatContact
```

At `BatContact`, the gameplay system evaluates:

```text
Ball Position
Bat Position
Bat Velocity
Bat Orientation
Timing Quality
Shot Intent
Power
```

and resolves the ball.

The animation should present the result; it should not become the authoritative source of physics.

---

# 18. Bowling Animations

Initial bowling library:

```text
Fast Bowling
Fast Yorker
Bouncer
Slower Ball
Off Spin
Leg Spin
Googly
Basic Wicketkeeping Receive
```

Each bowling action should have a clear:

```text
Run-up
Plant
Arm acceleration
Release
Follow-through
```

---

# 19. Ball Release Event

Bowling animations should contain:

```text
BallRelease
```

Conceptually:

```text
Bowling Animation
        ↓
BallRelease event
        ↓
Delivery parameters
        ↓
Ball physics
```

At the release event, gameplay determines:

```text
Pace
Line
Length
Swing
Seam
Spin
Accuracy
```

---

# 20. Motion Capture Without Professional Hardware

A phone camera is sufficient for early development.

Setup:

```text
Phone
 ↓
Tripod
 ↓
Full body visible
 ↓
Good lighting
 ↓
Plain background
 ↓
Perform action
 ↓
Record video
 ↓
AI video-to-motion
 ↓
Blender cleanup
```

Record:

- batting
- bowling
- catching
- throwing
- diving
- running
- celebrations

---

# 21. Motion Capture Recording Rules

For clean mocap:

1. Keep the entire body inside the camera frame.
2. Avoid extreme lighting changes.
3. Use a plain background.
4. Wear clothing that separates the body silhouette.
5. Avoid objects obscuring the hands.
6. Perform the movement several times.
7. Record multiple camera angles when possible.
8. Record individual actions rather than long sequences.

For example:

```text
BAD:
10-minute video containing batting + talking + walking + bowling

GOOD:
20-second video containing only a clean cover drive
```

---

# 22. AI Motion Capture

Possible workflow:

```text
Video
 ↓
Rokoko Video / equivalent
 ↓
Motion data
 ↓
FBX / BVH
 ↓
Blender
 ↓
Retarget
 ↓
Cleanup
 ↓
Unity
```

Official reference:

https://www.rokoko.com/

Alternative:

```text
Video
 ↓
MediaPipe-based pose estimation
 ↓
Pose data
 ↓
Retargeting
 ↓
Blender
```

The local/open-source route requires more technical setup but avoids depending on cloud quotas.

---

# 23. Motion Capture Cleanup

Raw mocap should always be cleaned.

Common problems:

```text
Foot sliding
Hand jitter
Finger noise
Hip instability
Knee popping
Incorrect shoulder rotation
Unwanted root motion
Equipment penetration
```

Cleanup workflow:

```text
Raw Mocap
   ↓
Remove noise
   ↓
Fix feet
   ↓
Fix knees
   ↓
Fix pelvis
   ↓
Fix shoulders
   ↓
Fix hands
   ↓
Align equipment
   ↓
Normalize timing
   ↓
Add events
```

---

# 24. Animation Retargeting

Maintain a single standard humanoid skeleton.

Conceptually:

```text
Motion Capture Skeleton
        ↓
Retarget
        ↓
CRICKET_PLAYER_SKELETON
        ↓
Player Variants
```

This allows multiple player appearances to share the same animation library.

---

# 25. Batting IK

Use IK to keep the player's hands connected to the bat.

```text
Left Hand ──┐
            ├── Bat Grip
Right Hand ─┘
```

The bat becomes an important rig target.

Use IK to improve:

- grip
- bat orientation
- stance
- follow-through
- shot direction

This reduces the number of bespoke animations required.

---

# 26. Foot IK

Use foot IK for:

```text
Batting stance
Bowling plant
Running
Stopping
Turning
Fielding
```

The objective is to reduce:

```text
Foot sliding
Foot penetration
Uneven contact with ground
```

---

# 27. Look-At IK

Use look-at behavior for:

```text
Batsman → ball
Bowler → batsman
Fielder → ball
Wicketkeeper → ball
```

The player should visually track the ball where appropriate.

---

# 28. Fielding Animation Strategy

Do not create an animation for every possible ball trajectory.

Use procedural movement + reusable actions.

```text
Ball Prediction
      ↓
Intercept Point
      ↓
Fielder Movement
      ↓
Run
      ↓
Plant
      ↓
Catch / Pickup
      ↓
Throw
```

Create reusable animations:

```text
Ground Pickup
Standing Catch
High Catch
Low Catch
Dive Left
Dive Right
Throw Overarm
Throw Underarm
Pickup and Throw
Reaction
```

---

# 29. Procedural Fielding

AI/gameplay determines:

```text
Where should the fielder go?
```

Animation determines:

```text
How should the fielder get there?
```

This separation is important.

```text
Ball trajectory
      ↓
AI intercept calculation
      ↓
Target position
      ↓
Navigation
      ↓
Animation
```

---

# 30. Wicketkeeping

Create:

```text
Keeper Idle
Keeper Ready
Receive Ball
Move Left
Move Right
Low Take
High Take
Dive
Stumping
Throw
Celebration
```

The stumping interaction should use IK where practical to align the gloves with the ball/stumps.

---

# 31. Running Between Wickets

Required animations:

```text
Sprint
Accelerate
Decelerate
Turn
Slide
Dive
Look Back
Call
Return
```

The turn animation is particularly important.

Players should not instantly rotate 180 degrees.

Use:

```text
Movement direction
      ↓
Turn anticipation
      ↓
Foot plant
      ↓
Rotation
      ↓
Sprint
```

---

# 32. Celebrations

Start with approximately 5–8 reusable celebrations:

```text
Fist Pump
Arms Raised
Jump
Point
Team Celebration
Bowler Celebration
Batsman Celebration
Wicket Celebration
```

Keep these modular so they can be reused across players.

---

# 33. Crowd

Do not individually model thousands of spectators.

Use:

```text
Low-poly spectator
        +
Variants
        +
Instancing
        +
Animation offset
```

Basic crowd animations:

```text
Idle
Clap
Stand
Cheer
Wave
Jump
```

Randomize:

```text
Rotation
Scale
Animation phase
Clothing
Color/material variation
```

This creates visual variety at much lower cost.

---

# 34. Materials and Textures

Use PBR materials:

```text
Base Color
Normal
Roughness
Metallic
Ambient Occlusion
```

Typical materials:

```text
Grass
Pitch
Leather
Wood
Metal
Plastic
Fabric
Concrete
Seat
Glass
Rubber
```

---

# 35. Texture Generation

AI can help create texture references, but raw AI images often contain:

- lighting baked into the image
- inconsistent perspective
- visible seams
- non-tileable patterns

Recommended workflow:

```text
AI texture concept
      ↓
Cleanup in GIMP
      ↓
Make tileable
      ↓
Generate PBR maps
      ↓
Material Maker / Blender
      ↓
Unity
```

---

# 36. Free Texture Sources

Poly Haven provides free HDRIs, textures and models.

https://polyhaven.com/

Use it for:

- environment textures
- HDRIs
- natural surfaces
- generic props

Always verify the current license and asset-specific terms before shipping assets in a commercial product.

---

# 37. Mobile Optimization

The target is mobile-first, so every asset should be created with optimization in mind.

Use:

```text
LOD
Texture compression
GPU instancing
Occlusion culling
Efficient materials
Low shader complexity
Object pooling
Addressables
```

Avoid:

```text
Huge 4K textures everywhere
Excessive unique materials
Unoptimized AI meshes
Thousands of unique GameObjects
Unnecessary high-poly geometry
```

---

# 38. LOD Strategy

Create multiple levels:

```text
LOD0
High detail

LOD1
Medium detail

LOD2
Low detail

LOD3
Very low / distant
```

Apply this especially to:

```text
Players
Stadium
Stands
Props
Crowd
Vegetation
```

Example starting targets for a player might be:

```text
LOD0: 30–50k triangles
LOD1: 15–25k
LOD2: 5–10k
LOD3: 1–3k
```

These are starting targets only. Final limits must be determined through profiling on representative target devices.

---

# 39. Texture Resolution Strategy

Example starting strategy:

```text
Hero player:
2048 or 1024

Normal player:
1024

Distant player:
512

Small prop:
512

Tiny prop:
256
```

Use texture atlases where appropriate.

Do not blindly give every object a 4K texture.

---

# 40. Unity Asset Folder Structure

Recommended structure:

```text
Assets/
└── Art/
    ├── Characters/
    │   ├── Base/
    │   ├── Players/
    │   ├── Clothing/
    │   ├── Hair/
    │   └── Equipment/
    │
    ├── Animations/
    │   ├── Locomotion/
    │   ├── Batting/
    │   ├── Bowling/
    │   ├── Fielding/
    │   ├── Wicketkeeping/
    │   ├── Running/
    │   ├── Reactions/
    │   └── Celebrations/
    │
    ├── Environment/
    │   ├── Stadium/
    │   ├── Pitch/
    │   ├── Stands/
    │   └── Props/
    │
    ├── Equipment/
    ├── Materials/
    ├── Textures/
    ├── VFX/
    └── Lighting/
```

---

# 41. Blender Project Structure

Keep Blender files organized similarly:

```text
Blender/
├── Characters/
├── Equipment/
├── Stadium/
├── Props/
├── Animation_Source/
├── Mocap/
├── Materials/
├── Textures/
└── Exports/
```

Never overwrite the original raw AI/mocap files.

Use:

```text
raw/
working/
final/
```

Example:

```text
Batting/
├── raw/
│   └── cover_drive_mocap.fbx
├── working/
│   └── cover_drive_clean.blend
└── final/
    └── cover_drive.fbx
```

---

# 42. Naming Convention

Use predictable names.

### Models

```text
SM_Stadium_Stand_A
SM_Cricket_Bat_01
SM_Cricket_Ball
SM_Stump_Set
SM_Floodlight
```

### Characters

```text
SK_Player_Base
SK_Player_Head
SK_Player_Hair
```

### Animations

```text
ANM_Bat_Idle
ANM_Bat_CoverDrive
ANM_Bat_CoverDrive_Early
ANM_Bat_CoverDrive_Late
ANM_Bowl_Fast
ANM_Bowl_Yorker
ANM_Field_Catch
ANM_Field_Throw
```

### Materials

```text
MAT_Grass
MAT_Pitch
MAT_Leather_Red
MAT_Leather_White
MAT_Wood_Bat
MAT_Player_Uniform
```

### Textures

```text
T_Grass_Base
T_Grass_Normal
T_Grass_Roughness
```

---

# 43. Animation Event Standard

Use gameplay events such as:

```text
BatContact
BallRelease
CatchPoint
ThrowRelease
StumpBreak
FootPlant
RunStart
RunTurn
```

The events should be treated as integration points between animation and gameplay.

---

# 44. Batting Event Example

```text
ANM_Bat_CoverDrive
        │
        ├── Start
        │
        ├── FootPlant
        │
        ├── BatContact
        │
        └── End
```

At `BatContact`:

```text
Gameplay
    ↓
Calculate contact quality
    ↓
Calculate exit velocity
    ↓
Calculate direction
    ↓
Resolve ball physics
```

---

# 45. Bowling Event Example

```text
ANM_Bowl_Fast
        │
        ├── RunUp
        ├── Plant
        ├── BallRelease
        ├── FollowThrough
        └── End
```

At `BallRelease`:

```text
DeliveryData
    ↓
Create / release ball
    ↓
Physics
```

---

# 46. Unity Import Pipeline

Recommended flow:

```text
Blender
 ↓
FBX
 ↓
Unity
 ↓
Model Import
 ↓
Rig Import
 ↓
Animation Import
 ↓
Avatar Configuration
 ↓
Animator
```

For characters:

```text
Rig Type = Humanoid
```

For equipment/environment:

```text
Rig Type = None
```

unless the asset has a specific animation requirement.

---

# 47. Animation Compression

Use Unity's animation compression carefully.

For gameplay-critical actions:

```text
Batting
Bowling
Catch
Throw
```

prioritize fidelity.

For distant/background animations:

```text
Crowd
Distant NPCs
Background movement
```

use stronger compression where acceptable.

Always verify visually.

---

# 48. Animator Architecture

A high-level Animator structure:

```text
Base Layer
│
├── Locomotion
│
├── Batting
│
├── Bowling
│
├── Fielding
│
└── Wicketkeeping

Upper Body Layer
│
├── Batting
├── Throwing
└── Reactions

Additive Layer
│
├── Breathing
├── Reactions
└── Minor movement
```

Avoid creating one giant state machine containing every possible gameplay combination.

Use layers, blend trees, parameters and Playables where appropriate.

---

# 49. Animation Parameters

Possible parameters:

```text
Speed
Direction
IsBatting
IsBowling
IsFielding
ShotType
TimingQuality
Power
IsRunning
IsKeeper
```

Keep gameplay state and animation state conceptually separate.

---

# 50. Shot Animation Selection

Example:

```text
ShotIntent
{
    shotType
    direction
    timing
    power
    footwork
}
```

Then:

```text
shotType
    ↓
Animation family
    ↓
timing
    ↓
animation variation
    ↓
IK adjustment
    ↓
gameplay physics
```

---

# 51. Avoid Animation Explosion

Do not create:

```text
20 shot types
×
3 timing types
×
8 directions
×
5 power levels
=
2400 animations
```

Instead use:

```text
10 base shot families
+
3 timing variants
+
IK
+
procedural orientation
+
gameplay parameters
```

This gives variety without an unmanageable asset count.

---

# 52. AI Prompt Strategy for 3D Objects

Use prompts that describe:

```text
Object
Shape
Material
Style
Camera
Background
Reference purpose
```

Example:

```text
A professional red cricket ball,
traditional leather construction,
clearly visible raised seam,
realistic proportions,
isolated object,
neutral studio lighting,
front three-quarter view,
clean reference for game asset modeling,
no text, no logo.
```

For a helmet:

```text
Professional cricket batting helmet,
modern protective grille,
realistic proportions,
isolated studio product reference,
front three-quarter view,
neutral background,
high detail,
suitable as reference for a low-poly game asset.
```

Use AI images primarily as references unless the output is suitable for direct texture/asset use.

---

# 53. AI Prompt Strategy for Stadium References

Example:

```text
Modern professional cricket stadium,
international cricket pitch,
large seating bowl,
four floodlight towers,
broadcast camera platform,
team pavilion,
electronic scoreboard,
advertising boards,
realistic sports-game environment concept,
clear architectural shapes,
daytime,
three-quarter aerial view,
no text.
```

Generate multiple concepts and choose one as the visual direction.

---

# 54. AI Prompt Strategy for Uniforms

Example:

```text
Professional cricket team uniform concept,
modern athletic jersey,
short sleeves,
subtle geometric pattern,
matching trousers,
clean sports branding areas,
front and side views,
flat studio presentation,
no player,
no logos,
game character clothing reference.
```

Do not copy real team trademarks or player likenesses unless you have the required rights.

---

# 55. AI-Assisted Face Generation

For a generic player system, faces can be generated or procedurally varied.

However:

- avoid real-person likenesses without permission
- avoid copyrighted celebrity likenesses
- keep facial topology compatible with your base mesh
- prioritize performance over extreme facial detail

The safest strategy is:

```text
Base head
+
Blend shapes
+
Hair variations
+
Material variations
```

---

# 56. Hair

Hair can be expensive on mobile.

Prefer:

```text
Hair cards
or
optimized low-poly meshes
```

rather than extremely dense strand-based hair.

Create a small library:

```text
Hair_01
Hair_02
Hair_03
Hair_04
Hair_05
```

Then randomly assign them.

---

# 57. Clothing

Use modular clothing.

```text
Uniform
├── Jersey
├── Trousers
├── Socks
└── Shoes
```

For national/team variants:

```text
Uniform_A
Uniform_B
Uniform_C
```

The body and skeleton remain the same.

---

# 58. VFX

Initial VFX:

```text
Ball trail
Dust
Grass particles
Bat impact
Boundary effect
Wicket impact
Crowd celebration effect
Rain
Smoke / atmosphere
```

Do not prioritize elaborate VFX before the core ball/bat interaction works.

---

# 59. Lighting

Create a small number of lighting presets:

```text
Day
Evening
Night
Cloudy
Floodlights
```

Avoid expensive lighting configurations on mobile.

Use baked/static lighting where practical and profile dynamic lighting carefully.

---

# 60. Asset QA Checklist

Every asset should pass:

```text
[ ] Correct scale
[ ] Correct orientation
[ ] Clean topology
[ ] No non-manifold geometry
[ ] UVs valid
[ ] Material assigned
[ ] Textures packed/available
[ ] Pivot correct
[ ] Collision strategy defined
[ ] LODs created if required
[ ] Naming convention followed
[ ] Mobile performance checked
[ ] Unity import tested
```

---

# 61. Animation QA Checklist

```text
[ ] Feet do not slide excessively
[ ] No obvious joint popping
[ ] Hands maintain contact with equipment
[ ] Bat follows intended path
[ ] Ball release is visually correct
[ ] BatContact event occurs correctly
[ ] BallRelease event occurs correctly
[ ] Root motion behavior is intentional
[ ] Animation transitions are smooth
[ ] IK does not create unnatural poses
[ ] Unity playback matches Blender
```

---

# 62. Recommended First Art Sprint

## Sprint 1 — Core Prototype

Create only:

```text
1 pitch
1 ball
1 bat
1 player
1 stadium blockout
1 batting stance
1 bowling action
1 run animation
1 fielding animation
1 catch
1 throw
```

Goal:

```text
Bowler
   ↓
Ball
   ↓
Batsman
   ↓
Shot
   ↓
Fielder
```

---

# 63. Sprint 2 — Batting

Add:

```text
Defense
Straight Drive
Cover Drive
Cut
Pull
Sweep
Loft
Miss
Edge
```

Add:

```text
Early
Good
Late
```

where useful.

Integrate:

```text
BatContact
```

---

# 64. Sprint 3 — Bowling

Add:

```text
Fast
Yorker
Bouncer
Slower
Off Spin
Leg Spin
```

Integrate:

```text
BallRelease
```

---

# 65. Sprint 4 — Fielding

Add:

```text
Pickup
Catch
Dive Left
Dive Right
Throw
Run to Ball
Stop Ball
```

Integrate:

```text
CatchPoint
ThrowRelease
```

---

# 66. Sprint 5 — Presentation

Add:

```text
Stadium detail
Crowd
Scoreboard
Lighting
Camera
Basic VFX
Celebrations
```

---

# 67. Sprint 6 — Mobile Optimization

Profile:

```text
CPU
GPU
RAM
Draw Calls
Triangles
Texture Memory
Animation Cost
Physics Cost
```

Test on actual representative Android devices.

Do not assume PC performance translates to mobile.

---

# 68. Full Character Production Pipeline

```text
AI Concept
   ↓
Character Reference
   ↓
AI 3D / Blender Base Mesh
   ↓
Topology Cleanup
   ↓
UV
   ↓
Texture
   ↓
Rig
   ↓
Weight Painting
   ↓
Mocap / Animation
   ↓
Retarget
   ↓
IK
   ↓
Animation Events
   ↓
LOD
   ↓
FBX
   ↓
Unity
```

---

# 69. Full Animation Production Pipeline

```text
Animation Requirement
       ↓
Reference Video
       ↓
Record Action
       ↓
AI Mocap
       ↓
Raw Motion
       ↓
Blender Retarget
       ↓
Cleanup
       ↓
Root Motion Setup
       ↓
Equipment Alignment
       ↓
IK
       ↓
Animation Event
       ↓
Export
       ↓
Unity Animator
       ↓
Gameplay Test
       ↓
Tune
```

---

# 70. Full Environment Production Pipeline

```text
AI Concept
    ↓
Architectural Reference
    ↓
Modular Blender Blockout
    ↓
High-detail assets where necessary
    ↓
Low-poly game meshes
    ↓
UV
    ↓
PBR Materials
    ↓
LOD
    ↓
Lighting
    ↓
Prefab
    ↓
Unity Scene
```

---

# 71. Suggested Production Team — Even for One Person

Treat yourself as multiple virtual roles:

```text
Role 1 — Art Director
Defines visual style.

Role 2 — 3D Artist
Creates models.

Role 3 — Technical Artist
Handles optimization, materials, rigs, LODs.

Role 4 — Animator
Creates/cleans animations.

Role 5 — Unity Developer
Integrates assets.

Role 6 — QA
Tests every asset.
```

AI tools can assist all six roles, but human review remains important for the final game-ready result.

---

# 72. AI Agent Workflow

For AI-assisted implementation, give the agent small tasks.

Good:

```text
Create the Unity import configuration for the batting animation set.
```

Good:

```text
Create an Animator Controller for locomotion and batting.
```

Bad:

```text
Build all cricket animations and integrate the whole game.
```

The smaller the task, the easier it is to:

- inspect changes
- catch mistakes
- revert changes
- test behavior
- maintain documentation

---

# 73. Recommended Agent Task Format

Each art/technical task should contain:

```text
TASK ID
Goal
Input assets
Expected output
Constraints
Naming rules
Performance requirements
Unity integration requirements
Validation steps
Definition of Done
```

Example:

```text
TASK: ART-010

Goal:
Create the first production-quality cover-drive animation.

Input:
Base humanoid rig.

Output:
ANM_Bat_CoverDrive.fbx

Requirements:
- Correct cricket stance
- Two-hand bat grip
- Natural footwork
- Clear bat contact moment
- Root motion configured
- BatContact event defined

Validation:
- No visible foot sliding
- Hands remain aligned
- Bat does not penetrate body
- Unity playback matches Blender

Definition of Done:
Animation imported into Unity and successfully triggered in the batting prototype.
```

---

# 74. Asset Versioning

Never destroy raw source files.

Use:

```text
v001
v002
v003
```

Example:

```text
ANM_Bat_CoverDrive_v001.blend
ANM_Bat_CoverDrive_v002.blend
ANM_Bat_CoverDrive_FINAL.blend
```

For serious development, Git/LFS or an equivalent asset versioning strategy should be considered for large binary files.

---

# 75. What Should Be AI-Generated vs Manual

## Strong AI candidates

```text
Concept art
Reference images
Simple props
Equipment prototypes
Texture concepts
Generic NPC variations
Motion capture extraction
Background assets
Variation generation
```

## Prefer Blender/manual

```text
Ball
Bat
Pitch
Stumps
Core player topology
Final character rig
Stadium modular kit
Collision geometry
Important gameplay equipment
Final animation cleanup
UVs
LOD setup
```

## Must be gameplay-controlled

```text
Ball physics
Bat-ball collision
Delivery parameters
Shot outcome
Fielding decisions
Run-out resolution
Wicket resolution
Scoring
Rules
Networking
```

---

# 76. The Critical Separation

The project should maintain this separation:

```text
ART
    ↓
Visual representation

ANIMATION
    ↓
Visual movement

GAMEPLAY
    ↓
Actual state

PHYSICS
    ↓
Physical resolution

RULES
    ↓
Cricket outcome

NETWORK
    ↓
Authoritative synchronization
```

Never allow an AI-generated animation or visual asset to become the authority for game state.

---

# 77. First Complete Vertical Slice

The first complete vertical slice should be:

```text
Scene
  ↓
Stadium
  ↓
Pitch
  ↓
Bowler
  ↓
Bowling animation
  ↓
BallRelease
  ↓
Ball physics
  ↓
Batsman
  ↓
Batting animation
  ↓
BatContact
  ↓
Shot physics
  ↓
Fielder
  ↓
Fielding animation
  ↓
Catch / pickup
  ↓
Rules
  ↓
Score
```

Once this works reliably, expand the content library.

---

# 78. Target Asset Library for the First Playable MVP

### Characters

```text
1 base male character
1 base female character if required by product scope
5 hair variants
5 body variations
5 uniform variants
5 shoe variants
basic equipment
```

### Environment

```text
1 stadium
1 pitch
1 pavilion
4 stands
4 floodlights
1 scoreboard
basic advertising
basic crowd
```

### Equipment

```text
3 bats
2 balls
1 helmet
1 glove set
1 pad set
1 wicket set
1 boundary rope
```

### Animations

```text
10 locomotion
10 batting
6 bowling
10 fielding
6 wicketkeeping
5 running-between-wickets
5 celebrations
5 reactions
```

This is enough to establish a convincing prototype without creating an enormous content burden.

---

# 79. Expansion After the MVP

Once the gameplay loop is proven:

```text
More shot types
More bowling actions
More fielding animations
More player bodies
More faces
More uniforms
More stadiums
More weather
More crowds
More celebrations
More equipment
```

Then:

```text
Career
Progression
Online multiplayer
11v11
Live events
Cosmetics
Advanced presentation
```

---

# 80. Final Recommended Pipeline

The recommended long-term pipeline is:

```text
                    CRICKET-GAME

                         │
                         ↓
                 ART DIRECTION
                         │
                         ↓
                 AI REFERENCES
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
         AI 3D Generation       Blender Modeling
              │                     │
              └──────────┬──────────┘
                         ↓
                    GAME MESH
                         ↓
              ┌──────────┴──────────┐
              ↓                     ↓
            UVs                 Materials
              │                     │
              └──────────┬──────────┘
                         ↓
                       RIG
                         ↓
               ┌─────────┴─────────┐
               ↓                   ↓
            Mixamo              AI Mocap
               │                   │
               └─────────┬─────────┘
                         ↓
                 Blender Cleanup
                         ↓
                     IK + Events
                         ↓
                    Optimization
                         ↓
                       FBX
                         ↓
                      UNITY
                         ↓
                Animator / Playables
                         ↓
                    GAMEPLAY
                         ↓
                  DEVICE TESTING
                         ↓
                    ITERATION
```

---

# 81. Golden Rules

1. **Blender is the central art hub.**
2. **Use AI to accelerate, not to blindly replace production workflows.**
3. **Generate reusable assets rather than one-off assets.**
4. **Create one strong base character before making many players.**
5. **Use modular stadium components.**
6. **Use video mocap for cricket-specific movement.**
7. **Clean every mocap result before shipping.**
8. **Use IK for hands, feet, equipment and ball tracking.**
9. **Use animation events for gameplay integration.**
10. **Keep physics and rules authoritative outside animation.**
11. **Optimize for mobile from the beginning.**
12. **Profile on real devices.**
13. **Keep raw AI/mocap files separate from final assets.**
14. **Use strict naming and versioning.**
15. **Build a playable vertical slice before creating hundreds of assets.**
16. **Do not make the project dependent on a single free AI service.**
17. **Verify licenses before commercial release.**
18. **Do not use real-player likenesses, team logos, or copyrighted assets without appropriate rights.**

---

# 82. First 20 Concrete Tasks

```text
ART-001  Create Blender project and units
ART-002  Create cricket pitch
ART-003  Create cricket ball
ART-004  Create cricket bat
ART-005  Create stump and bail set
ART-006  Create simple stadium blockout
ART-007  Create base player
ART-008  Rig base player
ART-009  Import player into Unity
ART-010  Create idle/walk/run
ART-011  Create batting stance
ART-012  Create cover drive
ART-013  Create defensive shot
ART-014  Create pull shot
ART-015  Create fast bowling action
ART-016  Add BallRelease event
ART-017  Add BatContact event
ART-018  Create basic fielder
ART-019  Create catch and throw
ART-020  Connect complete ball → bat → fielding loop
```

---

# 83. Definition of Success

The art pipeline should be considered successful when a developer can open Unity and see:

```text
A complete cricket scene

        ↓

A believable player

        ↓

A believable bowling action

        ↓

A correctly released ball

        ↓

A believable batting action

        ↓

A correctly timed bat-ball contact

        ↓

A believable ball trajectory

        ↓

A fielder moving toward the ball

        ↓

A catch / pickup / throw

        ↓

A visually coherent cricket sequence
```

At that point, the project has moved from:

> **"We have game documentation and AI-generated assets."**

to:

> **"We have a functioning visual/gameplay production pipeline."**

---

# 84. References

- Blender — https://www.blender.org/
- Mixamo — https://www.mixamo.com/
- Rokoko — https://www.rokoko.com/
- Poly Haven — https://polyhaven.com/
- GIMP — https://www.gimp.org/
- Material Maker — https://www.materialmaker.org/
- Unity — https://unity.com/

For AI 3D-generation services and local models, verify current availability, export formats, usage limits, and commercial licensing at the time of production.

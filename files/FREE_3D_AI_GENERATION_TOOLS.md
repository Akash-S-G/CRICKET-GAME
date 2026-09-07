# Free AI 3D Model Generation Tools - GitHub Frameworks + MCPs (2026)

> For cricket project. All tools below run free (open source self-host or free tier, no paid sub required per user). Locked free pipeline `animation_requirements.md:10` uses `Mixamo + Cascadeur + Plask/Rokoko Vision + Blender`.
> If tool is paid, listed only as fallback with free-tier note.

## 0. Quick Pick For Agent

| Your need | Use | Why | Cost |
|---|---|---|---|
| **Fast drafts <0.5s 6GB VRAM** | **TripoSR** `VAST-AI-Research/TripoSR` MIT | Single image -> textured 50-200K mesh, `<0.5s A100` `tasarim.ai 2024` | $0 self-host |
| **High quality 16GB PBR** | **TRELLIS 2** `microsoft/TRELLIS` MIT | `1536 res 20s 24GB` `3DAISTUDIO 2026` | $0 |
| **Image->3D unlimited** | **Hunyuan3D 2.1** `Tencent-Hunyuan/Hunyuan3D-2` Community | Best textures `cmarix.com` | $0 but check EU/UK/SK license |
| **No GPU, browser** | **3D AI Studio** `3daistudio.com` free tier | Runs Hunyuan+TRELLIS+Tripo+Rodin no setup | Free credits |
| **Blender-native + MCP** | **mcp-blender** `RFingAdam/mcp-blender` 218 tools AGPL | `text/image->3D Hyper3D/Meshy/Tripo/Hunyuan` in Blender via Claude | $0 `pip install mcp-blender` |
| **Unity agent 3D** | **Meshy MCP** `meshy-dev/meshy-mcp-server` | `text/image->3D` PBR + auto-rig + 500 anims `github.com/meshy-dev` | Free 200 credits/mo |

---

## 1. Open Source GitHub Frameworks (Self-Host Free, Unlimited)

### 1.1 Text/Image -> 3D Mesh (Main)

| Repo | Stars | License | Input | VRAM | Speed | Best for cricket |
|---|---|---|---|---|---|---|
| **TripoSR** `VAST-AI-Research/TripoSR` `github.com/VAST-AI-Research/TripoSR` | 6,933 | MIT | Single image | ~6GB | <0.5s | Fast `bat`/`stump` props `3.1` |
| **Stable Fast 3D** `Stability-AI/stable-fast-3d` | - | MIT | Single image | 6-8GB | <0.5s | Fast draft `3.1` |
| **TRELLIS 2** `microsoft/TRELLIS` `github.com/microsoft/trellis` `Awesome-3D-Generation` | - | MIT | Image -> 3D | 16GB+ 24GB rec | 20s 1536 | High `stadium` `3DAISTUDIO 2026` |
| **Hunyuan3D 2.1** `Tencent-Hunyuan/Hunyuan3D-2.1` | - | Community (EU/UK/SK limit `3DAISTUDIO`) | Image | 16GB+ | seconds | High texture `player` `cmarix.com` |
| **Hunyuan3D 2.5 / Omni** `Hunyuan3D-2` `Hunyuan3D-Omni` | - | Community | Image/Text | 16GB+ | seconds | PBR hero `firethering 2026` |
| **InstantMesh** `TencentARC/InstantMesh` | - | MIT | Single image | 8GB+ | sec | Game `40` |
| **Hi3DGen** `research/Hi3DGen` `BunnySoCrazy/Awesome-3D` | - | MIT | Image | 16GB+ | sec | Geometric `SPAR3D` |
| **Zero123++** `SUDO-AI-3D/Zero123Plus` | - | MIT | Image | 8GB+ | sec | View synthesis |
| **Shap-E** `openai/shap-e` | - | MIT | Text/Image | 6GB | 10s | Text `tripo` `virtualcoders.net` |
| **GET3D** `nv-tlabs/GET3D` | - | NV | Text | 16GB | sec | Generative `firethering` |
| **One Model To Rig All UniRig** `VAST-AI-Research/UniRig` SIGGRAPH25 | - | MIT | Mesh | - | - | `Skeleton rig` `BunnySoCrazy` |
| **RigAnything** `Isabella98Liu/RigAnything` | - | MIT | Mesh | - | - | Template-free rig |

**Install TripoSR (example):**
```bash
git clone https://github.com/VAST-AI-Research/TripoSR
pip install -r requirements.txt
python run.py --image input/bat.jpg --output bat.obj # 0.5s 6GB
# Gradio: python gradio_app.py
```

**Install Hunyuan/TRELLIS locally via Modly wrapper:**
```bash
# Modly free desktop local AI per modly3d.app
# Download modly3d.app -> Install extension Hunyuan3D 2 / TRELLIS2 / TripoSR
# Runs offline on NVIDIA GPU no cloud per modly3d.app
```

### 1.2 Aggregator Lists

- **BunnySoCrazy/Awesome-3D-Generation** `github.com/BunnySoCrazy/Awesome-3D-Generation` - curated TRELLIS/Hunyuan/TripoSR/UniRig/RigAnything/MagicArticulate/Make-It-Animatable `2025-10-28`
- **Awesome 3D topics** `github.com/topics/3d-generation` `CVPR Spotlight` structured latents

---

## 2. Free Hosted Tiers (No GPU, Browser)

| Tool | Free Offering | Input | Commercial | Best for | Ref |
|---|---|---|---|---|---|
| **3D AI Studio** `3daistudio.com` free | Free tier + library | Text/Image | Yes | All-round `3DAISTUDIO 2026` | `3daistudio.com/blog/best-free...` |
| **Meshy** `meshy.ai` | 200 credits/mo free | Text/Image | Free CC BY 4.0, Pro commercial | Game PBR 4K + auto-rig 500 anims `meshy-dev/game-asset-pipeline` | `github.com/meshy-dev` |
| **Tripo** `tripo3d.ai` `Tripo Studio` | Free credits monthly | Text/Image/multi | Yes | Fast clean topology | `3d-agent 2026` |
| **Luma Genie** | Free limited | Text | Often limited | Concepts | `3DAISTUDIO` |
| **Rodin trial** `hyper3d.ai` | Trial credits | Text/Image | High | Hero | `3d-agent` |

> Meshy free 200 credits/mo, Pro 1000/mo `June 2026` `github.com/meshy-dev/game-asset-pipeline`. Outputs GLB/FBX/OBJ/STL/USDZ/BLEND PBR 1K-300K tri `meshy-dev`.

---

## 3. MCP Servers - Agent Can Call Directly (Claude/Cursor)

### 3.1 3D Generation MCPs

| MCP | Repo | Tools | Backends | Install | Cost |
|---|---|---|---|---|---|
| **Blender MCP** `ahujasid/blender-mcp` + **mcp-blender** `RFingAdam/mcp-blender` 218 tools | `github.com/RFingAdam/mcp-blender` AGPL 13 stars | `3D modeling, AI generation (Hyper3D Rodin, Meshy, Tripo, TripoSR, Stable Fast 3D, Hunyuan3D, ComfyUI), render_multi_angle, Poly Haven` | Hyper3D/Meshy/Tripo/TripoSR/Hunyuan + self-refine `render->analyze->refine` via Ollama vision | `pip install mcp-blender` `uvx blender-mcp` `blendermcp.org` | $0 local GPU |
| **Meshy MCP Server** `meshy-dev/meshy-mcp-server` | `github.com/meshy-dev/meshy-mcp-server` | `text-to-3D, image-to-3D, AI texturing, rig, 500 anims` | Meshy API | `npx meshy-mcp-server` `meshy.ai/mcp` | Free 200/mo |
| **3d-agent-mcp** `teslaproduuction/3d-agent-mcp` | `github.com/teslaproduuction/3d-agent-mcp` | `text->3D Hunyuan3D/TripoSR/FLUX + cloud APIs + Meshy` | Hunyuan/TripoSR/FLUX + cloud | `npm i 3d-agent-mcp` | $0 local + cloud |
| **Blender MCP official** `blender-mcp` `blendermcp.org` | `blendermcp.org` | `218 Blender tools, AI 3D Hyper3D/Hunyuan` | Rodin/Hunyuan | `uvx blender-mcp` in `claude_desktop_config.json: blender -> uvx blender-mcp` | $0 |

**Blender MCP config (agent):**
```json
{
  "mcpServers": {
    "blender": { "command": "uvx", "args": ["blender-mcp"] },
    "meshy": { "command": "npx", "args": ["-y", "meshy-mcp-server"] }
  }
}
```
Blender add-on `blender_mcp_addon.zip` `Edit > Preferences > Add-ons > Install` `github.com/RFingAdam/mcp-blender`.

### 3.2 Unity/Game MCPs For Pipeline

| MCP | What agent does | Repo | Cost |
|---|---|---|---|
| **Unity MCP Official** `com.unity.ai.assistant 2.17` | Scene/GameObject/Component/PlayMode `unity.com/blog/unity-ai-mcp-how-to-get-started` `2026-05-11` | `docs.unity3d.com` | Free |
| **AnkleBreaker unity-mcp-server** | 268 tools `Scene/Physics/Terrain/ShaderGraph/Profiling/Animation` `github.com/AnkleBreaker-Studio/unity-mcp-server` 406 stars | `AnkleBreaker` | Open license |
| **IvanMurzak Unity-MCP** | 71 tools `TECH_STACK.md:5` | `IvanMurzak/Unity-MCP` | MIT |
| **Godot MCP** `Skiln 2026` | Nodes/GDScript 12min | `skiln.co` | Free |
| **Blender MCP** | 218 meshes/materials/Python `skiln.co 2026` | `blender-mcp` | Free |

> `Skiln 2026` ranking: Unity MCP (Bridge) vs Godot vs Blender - all free open source `skiln.co/blog/best-game-development-mcp-servers-2026`.

### 3.3 ComfyUI / Local Pipelines

- **ComfyUI + 3D nodes** `ComfyUI-3D-Pack` runs Hunyuan/TRELLIS locally, feeds `mcp-blender deploy/comfyui` `RFingAdam`.
- **Modly** `modly3d.app` `github.com/lightningpixel/modly` MIT Windows/Linux offline wrapper for Hunyuan/TripoSR/TRELLIS - no Python setup.

---

## 4. GitHub Frameworks For Rig / Animation

| Repo | For cricket | License |
|---|---|---|
| `UniRig` `VAST-AI-Research/UniRig` SIGGRAPH25 `BunnySoCrazy` | One rig for diverse skeletons `player` | MIT |
| `RigAnything` `Isabella98Liu/RigAnything` | Template-free `bat` rig | MIT |
| `MagicArticulate` `Seed3D/MagicArticulate` CVPR25 | Articulation-ready `stumps` | MIT |
| `Make-It-Animatable` `jasongzy/Make-It-Animatable` CVPR25 | Anim-ready character | MIT |

---

## 5. Recommended Free Stack For This Cricket Project

**Agent runs local, no sub (per user):**

1. **Concept:** `3D AI Studio` free `Text: cricket batsman T-pose` -> download FBX Mixamo-compatible `3DAISTUDIO`.
2. **Self-host refine:** `TripoSR` 6GB `<0.5s` for `ball` `stumps` `bat` props `VAST-AI-Research`.
3. **Hero:** `Hunyuan3D 2.1` or `TRELLIS 2` 16GB for `stadium` `player` `cmarix.com`.
4. **Rig:** `Mixamo` auto-rig `1.8m` `features/02/00_rig_source_and_naming.md:16` + `UniRig` fallback.
5. **Agent create:** `Claude + blender-mcp` `uvx blender-mcp` + `meshy-mcp-server` `npx` -> `generate 3D: cricket bat low poly` in Blender without UI `blendermcp.org`.
6. **Validate:** `ValidateClipNames.cs` `features/02/00_rig_source_and_naming.md:44` + `ajv` `files/camera_config_schema.json:1`.

**Install all free at once (build machine):**
```bash
pip install mcp-blender
npm i -g meshy-mcp-server
git clone https://github.com/VAST-AI-Research/TripoSR && pip install -r TripoSR/requirements.txt
git clone https://github.com/Tencent-Hunyuan/Hunyuan3D-2 && pip install -r Hunyuan3D-2/requirements.txt
# Modly offline
wget https://github.com/lightningpixel/modly/releases/latest/download/Modly.exe
```

## 6. Credits / Limits To Know

- Hosted free tiers need account credit tracking, personal vs commercial `3d-agent 2026` `Free CC BY 4.0 vs Pro commercial` `meshy-dev`.
- Open source needs `6-24GB VRAM` `cmarix.com` table `TRELLIS 16GB+`.
- All above are permissive MIT/Apache/Community, no per-model cost if self-host `firethering.com 2026-03-06` 5 tools list.

---

## 7. References

- `BunnySoCrazy/Awesome-3D-Generation:2025-10-28` Hub
- `cmarix.com:2026-08-19` 7 best MIT `TRELLIS 2` `Hunyuan 2.1` `TripoSR`
- `modly3d.app` MIT local `Hunyuan/TRELLIS/Tripo`
- `3daistudio.com:2026-06-03` free credits `Meshy/Tripo/Hunyuan/TRELLIS` comparison
- `RFingAdam/mcp-blender:2026-02-05` 218 tools
- `meshy-dev/meshy-mcp-server` + `meshy-ai:200 credits`
- `VAST-AI-Research/TripoSR:6,933` `<0.5s`
- `Tencent-Hunyuan/Hunyuan3D-2.1` `Hunyuan3D-Omni` `2025`
- `Skiln 2026-06-20` Unity/Godot/Blender MCP rank
- `firethering 2026-03-06` Trellis open unknown

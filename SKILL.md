---
name: ai-blender-web-showcase
description: "Use this when the user wants to create a public-facing interactive 3D showcase for any product, object, vehicle, sports figure, or concept: search Sketchfab/Sketchfab for 3D models, operate Blender via MCP, prepare GLTF/GLB, and build a Three.js/WebGL interactive webpage. Supports single-product hero pages AND multi-model galleries (click-to-expand, drag-to-rotate). Trigger on: Codex 操控 Blender 做 3D 展示网页, 3D 建模加网站展示, 用 Three.js 展示模型, 产品 3D 展示页, 交互式 3D 展厅, World Cup 3D 球星, 多模型画廊, or similar. Do NOT use for recording, editing, Douyin copywriting, or publishing — use creator-demo-recording for that."
metadata:
  short-description: Blender MCP + Three.js 3D showcase (single & multi-model)
version: 2.1
---

# AI Blender Web Showcase

## Core Outcome

Deliver a working interactive 3D showcase: search/download models → Blender processing via MCP → GLTF/GLB export → Three.js/WebGL page → visual verification.

Two supported page patterns:
- **Single-model hero**: one product, full-screen 3D, drag-to-rotate, interactive parts
- **Multi-model gallery**: grid/circle of models, click-to-expand, drag-to-rotate, Esc to go back

## How Users Should Invoke This Skill

Use this skill when the user asks for an end-to-end 3D webpage, not only a Blender model or only a webpage. Typical prompts:

- "用 Codex 操控 Blender 做一个汽车 3D 展示网页"
- "找模型、处理模型，然后用 Three.js 做可交互产品页"
- "做一个 8 个球星的 3D 画廊，点击放大、拖拽旋转"
- "把这个产品做成 WebGL 展示页，可以打开局部细节"

If the request is only video recording, publishing copy, or Douyin/TikTok packaging, hand off to `creator-demo-recording` instead. If the request is only frontend polishing after assets already exist, use `frontend-design` plus `webapp-testing`.

## Blender MCP Setup

Blender is controlled via the Blender MCP bridge (stdio MCP server → TCP port 9876 → Blender).

**Prerequisites:**
1. Blender must be running with the BlenderMCP addon enabled (View3D sidebar → BlenderMCP → Start Server)
2. The bridge is configured in `.claude.json` as an MCP server named `blender`
3. Bridge path: `scripts/blender_mcp_bridge.py` in the project root

**Available MCP Tools:**

| Tool | Purpose |
|---|---|
| `blender_execute_code` | Execute arbitrary Blender Python — create/modify objects, materials, animations, export |
| `blender_search_sketchfab` | Search Sketchfab for downloadable 3D models |
| `blender_download_sketchfab_model` | Download + import a Sketchfab model (auto-normalize size) |
| `blender_get_scene_info` | Read scene state (objects, materials) |
| `blender_get_object_info` | Read object details (location, scale, bounding box, materials) |
| `blender_get_viewport_screenshot` | Capture viewport for visual verification |
| `blender_polyhaven_search` | Search PolyHaven for HDRIs/textures |
| `blender_polyhaven_download` | Download PolyHaven asset into scene |

## Default Technical Stack

- **Blender MCP**: via bridge → TCP `localhost:9876`. Use `blender_execute_code` for all Blender Python operations.
- **Sketchfab API**: search and download 3D models via MCP tools (requires API key configured in BlenderMCP addon).
- **GLB/GLTF**: preferred web format. Keep object names and hierarchy for Three.js targeting.
- **Three.js + WebGL**: realtime rendering, lighting, camera, responsive canvas, GLTFLoader.
- **GSAP**: smooth model/camera motion, expand/collapse, view transitions.
- **Vite**: dev server and build tooling.

Optional stack choices:
- **React** when the app needs routed views, reusable UI state, or a richer control panel.
- **Vanilla JS** when the goal is a compact single-page showcase.
- **Draco / meshopt compression** when GLB size is too large for web delivery.
- **KTX2 / Basis textures** when texture payload size is the bottleneck.

## Related Skills To Combine

- `frontend-design`: use for the surrounding webpage, UI controls, responsive layout, and visual polish.
- `webapp-testing`: use for Playwright verification after the Vite server is running.
- `browser:browser`: use to open localhost, inspect interactions, and capture screenshots in the Codex in-app browser.
- `gsap`: use when animation timing, timelines, staggered reveals, or motion polish are central to the result.
- `creator-demo-recording`: use after the 3D page works, when the user wants recording, social clips, titles, or publishing copy.
- `vehicle-asset-automation` / `vehicle-model-asset-processing`: use for strict automotive asset cleanup, naming, O3D/editor readiness, or production-grade vehicle model packaging.
- `imagegen`: use only for bitmap support assets such as background plates, poster images, texture concepts, or placeholder product visuals. Do not use it as a substitute for real 3D model processing.

## External Asset Sources

Prefer real downloadable 3D assets when specificity matters. Always inspect license terms and keep credit/source notes.

- **Sketchfab**: primary source for downloadable character, vehicle, product, prop, and fan-made models. Use MCP search/download tools when available.
- **Poly Haven**: HDRIs, textures, and some environment assets; useful for lighting and material realism.
- **Google Poly archives / community mirrors**: acceptable only when license/source is clear.
- **CGTrader / TurboSquid / Fab / BlenderKit**: useful for paid or higher-fidelity models; download manually when MCP integration is unavailable.
- **Manufacturer/product press kits**: useful for references, colors, dimensions, and product imagery; do not assume 3D redistribution rights.
- **Procedural Blender generation**: fallback for stylized explainers, abstract concepts, or when no licensed model is available.

## Mandatory Planning Gate

1. **Showcase specification.**
   - Target object(s), fidelity level, brand/style cues.
   - Page pattern: single-model or multi-model gallery.
   - Interaction contract: what click/drag/scroll does, what must never happen.
   - For multi-model: how many items, gallery layout (circle/grid), expand behavior.

2. **Gap audit.** Compare request against available models, references, current state.
   - For sports figures / people: Sketchfab is the primary source. Search with player name + "soccer" / "football".
   - Name concrete gaps (wrong body proportions, missing team kit details, no pose).

3. **Technical plan.**
   - Blender plan: source model, inspect hierarchy, materials, scale normalization, pose adjustment, export path.
   - Web plan: Three.js scene, camera strategy, interaction mode, GSAP animations, layout.

4. **Task breakdown.** Execute in order: find model → Blender process → export → webpage → verify.

## Execution Workflow

### 1. Find and source 3D models

- **Use `blender_search_sketchfab`** to search for models (e.g. "Mbappe soccer", "Messi football player").
- **Use `blender_download_sketchfab_model`** to download and import. Always pass `normalize_size: true, target_size: 2.0` for consistent scaling across multiple models.
- If Sketchfab has no suitable model, use PolyHaven or create a stylized procedural model.
- For sports figures: prioritize models with recognizable team kit, pose, and proportions.
- **Credit third-party assets** when required by license. Record source URLs.

### 2. Process in Blender via MCP

- Use `blender_execute_code` to:
  - Inspect imported model hierarchy (`get_scene_info` / `get_object_info`)
  - Clean up unnecessary objects (empties, cameras, lights from import)
  - Adjust materials: team colors, metallic accents, skin shading
  - Add lighting: three-point setup (key, rim, ambient)
  - Add ground shadow plane if needed
  - Pose adjustment if model is in T-pose
- Use `blender_get_viewport_screenshot` to verify before export.
- **For multi-model projects**: process each model to consistent scale (~2m height for human figures).

### 3. Export GLB

```python
# blender_execute_code example
import bpy
bpy.ops.object.select_all(action='SELECT')
bpy.ops.export_scene.gltf(
    filepath='/path/to/output/player_name.glb',
    export_format='GLB',
    export_apply=True,
    export_image_format='JPEG'
)
```

### 4. Build the interactive webpage

**Single-model hero page:**
- Full-screen Three.js canvas.
- Model at center, product-drag rotation (rotate model root, not camera).
- GSAP for initial reveal, hover states, UI overlays.
- Buttons for: rotate reset, detail focus, color/material switch.

**Multi-model gallery (new in v2):**
- Arrange models in a circle or grid layout (typical radius: 4-5 units for 8-10 models).
- Each model slowly auto-rotates when idle.
- **Click to select**: selected model rises (y+1.5), scales up (×2), camera dollies in.
- **Drag to rotate** selected model (rotate model root, clamp x-rotation ±60°).
- **Scroll to zoom** camera distance (range: 2-8 units).
- **Esc / click outside** to deselect and return to gallery.
- Unselected models dim to 30% opacity during selection.
- Player info overlay fades in on selection (name, country, number).

### 5. Verify visually

- Open in browser at `localhost:3000` (Vite dev server).
- Check: all models visible, no clipping, lighting reads well.
- Check: click-to-select, drag rotation, scroll zoom, Esc deselect.
- Check: info overlay doesn't block interaction.
- Screenshot the most impressive angle for social sharing.

### 6. Hand off for recording

- Provide: local URL, Blender file path(s), model asset paths, key interactions.
- Switch to `creator-demo-recording` skill for Douyin publishing workflow.

## Multi-Model Gallery Pattern (World Cup Example)

```
Gallery state:
  - 8 player spheres arranged in circle (radius 4.5, y=0)
  - Each sphere: team color + gold ring + jersey number
  - Idle auto-rotation at 0.3 rad/s
  - Subtle float animation (sin wave, ±0.2 amplitude)

Selected state:
  - Selected player rises to y=1.8, scales to 1.6x
  - Camera moves to (player.x, player.y+0.5, player.z+3)
  - Player rotates via drag (horizontal: rotate Y, vertical: rotate X clamped)
  - Scroll adjusts camera distance (2–8)
  - Other players fade to 30% opacity

Transitions:
  - GSAP power2.out/power3.out for all camera/position/scale tweens
  - Duration: 0.8s position/scale, 1.2s camera
```

## Quality Bar

- Models must be recognizably the requested objects (proportions, colors, key features).
- For sports figures: team colors + number must be visible.
- Interaction must work without explanation (hint text fades on first interaction).
- All models must be visible in gallery view (no clipping, no off-screen).
- Final delivery includes: file paths, local dev URL, interaction list.

## Common Failure Checks

- Do not start coding before writing a specification and technical plan.
- Do not present generic spheres as specific players when real models are available on Sketchfab.
- Do not let models clip into each other in gallery view (adjust radius for model count).
- Do not let the selected model fill the entire screen (keep min camera distance).
- Do not let drag rotation go wild (clamp X rotation, apply inertia damping 0.95).
- Do not lose the camera target during selection/deselection transitions.
- Do not block the 3D canvas with oversized UI overlays.
- Do not skip asset license notes for Sketchfab/third-party models.
- Do not assume Blender MCP is connected — verify with `blender_get_scene_info` first.

# AI Blender Web Showcase

Codex skill for building interactive 3D showcase webpages with Blender MCP, real 3D assets, GLB/GLTF, Three.js, WebGL, GSAP, and Vite.

It is designed for end-to-end requests such as:

- creating a 3D product showcase page
- building a WebGL car or object hero page
- making a multi-model gallery with click-to-expand and drag-to-rotate interactions
- sourcing external 3D models, processing them in Blender, exporting GLB, and verifying the final web page

## Install

Copy this repository into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/lukaizj/ai-blender-web-showcase.git ~/.codex/skills/ai-blender-web-showcase
```

Restart Codex or reload skills after installation.

## Usage

Example prompts:

```text
用 ai-blender-web-showcase 做一个汽车 3D 展示网页
```

```text
找一个产品 3D 模型，用 Blender 处理，然后做成 Three.js 可交互网页
```

```text
做一个 8 个世界杯球星的 3D 画廊，点击放大、拖拽旋转、滚轮缩放
```

## Technical Stack

- Blender MCP for scene processing, materials, lighting, export, and screenshots
- Sketchfab for downloadable 3D models when available
- Poly Haven for HDRIs, textures, and environment assets
- GLB/GLTF as the preferred web asset format
- Three.js and WebGL for realtime 3D rendering
- GSAP for camera movement, selection transitions, and UI animation
- Vite for local development and build tooling

Optional additions:

- React for richer UI state or routed experiences
- Draco or meshopt compression for large GLB files
- KTX2/Basis textures for texture-heavy projects

## Related Codex Skills

- `frontend-design`: webpage layout, UI controls, responsive polish
- `webapp-testing`: Playwright checks and screenshots
- `browser:browser`: open and inspect localhost in the Codex browser
- `gsap`: animation timing and motion design
- `creator-demo-recording`: recording workflows and social publishing copy
- `vehicle-asset-automation`: strict vehicle asset cleanup and validation
- `vehicle-model-asset-processing`: end-to-end automotive 3D model processing
- `imagegen`: bitmap support assets such as backgrounds or texture concepts

## External Asset Sources

Use licensed assets and keep source notes:

- Sketchfab
- Poly Haven
- CGTrader
- TurboSquid
- Fab
- BlenderKit
- manufacturer press kits for visual references

If no suitable asset is available, the skill can fall back to stylized procedural Blender generation.

## Repository Contents

- `SKILL.md`: Codex skill instructions
- `agents/openai.yaml`: UI metadata for Codex skill listings
- `references/tesla-model3-case.md`: single-model product showcase notes
- `references/worldcup-case.md`: multi-model sports gallery notes

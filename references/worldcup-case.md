# World Cup 3D Stars Case Notes

Use these notes when building multi-player sports figure showcases.

## Story Arc

1. Start with a real reference target: World Cup 2026 star players.
2. Source 3D models from Sketchfab (search "[player] soccer" or "[player] football").
3. Process each model in Blender via MCP:
   - import via `blender_download_sketchfab_model` (normalize_size=true, target_size=2.0)
   - adjust materials to team colors if needed
   - clean up hierarchy, remove extraneous objects
   - add three-point lighting
   - export as GLB to `web/public/models/`
4. Build Three.js multi-model gallery page:
   - circle/grid layout with 8-10 players
   - click-to-select with rise + enlarge animation
   - drag-to-rotate selected player
   - scroll-to-zoom camera
   - Esc to deselect
5. Verify:
   - all players visible and correctly scaled
   - team colors and numbers readable
   - interaction responsive at 60fps
   - mobile touch support
6. Hand off for recording:
   - local dev URL (Vite, port 3000)
   - Blender files per player
   - model assets with license credits

## Interaction Rules

For multi-player gallery:
- Gallery view: auto-rotate each model slowly, float idle animation
- Click player → camera dollies in, player rises, other players dim
- Drag rotates the selected player (rotate model Y for horizontal, X for vertical, clamped ±60°)
- Scroll adjusts camera distance (2–8 units)
- Esc or click empty space → return to gallery
- GSAP power2.out for position/scale, power3.out for camera moves

## Player Data Schema

```js
{
  id: 'mbappe',        // unique slug, used as filename and DOM id
  name: 'MBAPPÉ',      // display name (all caps for impact)
  number: 10,          // jersey number
  country: '🇫🇷 France', // flag + country
  color: '#1a237e',    // team primary color (used for material/ring)
}
```

## Blender Processing Notes

- All models normalized to ~2m height for consistent gallery scale.
- Three-point lighting: key (5, 8, 5), rim (-3, 2, -5), ambient.
- Ground shadow plane at y=-0.5 for depth cue.
- Export as GLB with JPEG textures, apply modifiers, include selected only.

## Douyin Angle

If recording for Douyin:
- "世界杯来了，我用 Codex + Blender 做了一个 3D 球星画廊"
- "点击球星放大，拖拽旋转，滚轮缩放——普通人用 AI 也能做"
- "技术栈：Blender MCP / Three.js / WebGL / GSAP"
- Highlight the multi-model interaction: "8 个球星，一键切换"

Avoid:
- claiming from-scratch character modeling if Sketchfab models were used
- hiding asset credits when asked

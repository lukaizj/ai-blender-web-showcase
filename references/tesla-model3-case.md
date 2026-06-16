# Tesla Model 3 Case Notes

Use these notes when the user asks to repeat or adapt the Tesla Model 3 style workflow.

## Story Arc

1. Start with a real reference target: Tesla Model 3, black warrior/dark tech style.
2. Use a real Model 3 3D asset rather than a generic sedan.
3. Process the model in Blender:
   - import GLTF
   - inspect mesh hierarchy
   - assign dark metallic paint, smoked glass, black wheels, cabin materials
   - identify door dummy/root nodes
   - add visual markers for door hinge nodes
   - orbit the model for 360 checks
   - show interior details through open doors
4. Export GLB for web.
5. Build Three.js page:
   - real-time WebGL render
   - OrbitControls 360 drag
   - GSAP camera and door animation
   - buttons for open doors, cabin focus, profile/reset views
6. Verify:
   - full car visible, including lower body and wheels
   - front/rear doors swing outward around vertical pivots
   - interior view actually shows seat/screen/steering wheel
7. Prepare handoff notes for the recording workflow if requested:
   - Blender file path
   - website local URL
   - selected nodes/wireframe moments worth showing
   - 360 orbit and open-door/interior interactions

## Door Direction Rule

For normal Model 3 doors:
- Front and rear doors swing outward.
- Hinges are vertical.
- The door should move away from the cabin, not into the car body.
- In the webpage, compute/verify opening direction from geometry when possible. A robust check is to test positive and negative hinge rotations and choose the rotation that moves the door center farther away from the model center on the side axis.

## Douyin Angle

If the user asks for recording, title, or publishing copy, switch to `creator-demo-recording`. Give that skill these angles:
- "不是只写代码，是 Codex 参与 Blender + 前端完整制作"
- "普通人会描述需求，也能做 3D 产品展示页"
- "技术栈真实：Blender / GLTF / Three.js / WebGL / GSAP"

Avoid:
- claiming from-scratch professional automotive modeling if a third-party model asset was used
- hiding license/asset credit when asked for source details

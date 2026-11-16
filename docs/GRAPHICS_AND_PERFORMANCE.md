# Graphics, Browser Support and Performance Plan

**Status:** Proposed targets, not measured results.

## Rendering contract

Use physically based materials and coherent lighting. WebGPU is preferred where available; attempt WebGL fallback automatically. WebGPU support depends on browser/OS/GPU and secure contexts. Do not promise ultra settings or identical performance on all devices.

## Quality tiers

| Preset | Intended class | Main tradeoff |
| --- | --- | --- |
| Low | Entry mobile / fallback | Resolution scale, shadow counts, reflection cost and effects reduced |
| Medium | Typical mobile | Selected texture/geometry detail and controlled shadow budget |
| High | Powerful phones / mid desktops | Better reflections, LOD, foliage and edge quality |
| Ultra | High-end desktops | Highest tuned assets and effects after actual profiling |
| Auto | Every supported device | Runtime capability selection, observed frame-time adaptation |

Every preset must keep the same validated physics timestep, bike data and race rules. It is acceptable for visual fidelity to vary dramatically.

## Provisional numerical objectives

- 60 FPS **target** on selected capable desktop reference hardware (16.7 ms average frame budget).
- 30 FPS **minimum target** on selected supported mobile reference devices during representative racing (33.3 ms average frame budget); 60 FPS preferred where hardware allows.
- Avoid persistent multi-second stalls, large GC spikes, and input latency that makes control difficult.
- Startup download size and device memory caps are TBD after asset selection and network profiling. Do not invent a fixed universal MB limit.
- Define percentile frame times (median, p95, p99), thermal stability and long-session performance in Phase 00/05 before making release claims.

## Reference hardware matrix

Record **real model, OS, browser version, GPU/browser renderer**, viewport, screen density, preset, average/p95 frame time, load time, memory notes, input response and major issues.

At minimum test: one budget Android phone, one midrange Android phone, one capable desktop/laptop and one fallback/WebGL browser. Broader public support claims require expanded matrix; emulators do not replace physical thermal/multitouch testing.

## Rendering work items

1. Baseline scene instrumentation and debug overlay.
2. Asset loading strategy: compressed textures, streaming, GLB validation, loading indicator, graceful failures.
3. Reuse of geometry/materials, GPU instancing for props, view/frustum culling.
4. LOD models for bikes, scenery and roadside objects.
5. Shadows, reflections and postprocess tuned against real costs.
6. Device-sensitive resolution scaling and stable adaptation hysteresis.
7. Session endurance tests, pause/resume and context loss handling.
8. Independent gameplay tests to ensure frame rate never changes engine behavior.

## Visual quality review

Screenshots from fixed camera checkpoints under the same scene lighting should be compared at each tier. Check texture stretching, tire contact, model scale, road seams, vegetation pop-in, shadow flicker, UI clarity and camera clipping. Asset quality and performance must be reviewed together.

## Audio browser note

Audible Web Audio contexts often require an explicit player action to start or resume. Present sound enabled as a preference but initialize/resume after a supported gesture; offer mute/volume. Do not claim unrestricted autoplay.

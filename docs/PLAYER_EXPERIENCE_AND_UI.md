# Player Experience and UI Art Direction

**Status:** Proposed UX foundation, not a designed or implemented game.

## Experience intent

The game should feel like an intentional, professional motorsport product. Design is driven by motorcycles, tracks, instrumentation, readable driving feedback and animation linked to gameplay. Avoid generic AI-styled panels, neon glows, random 3D floating icons and excessive motion unrelated to the bike.

## Core screens (implement only when their phase is authorized)

1. Compatibility and loading state: renderer status, progress, actionable errors.
2. Home/lobby: **Ride Now** primary action, garage, settings, controls help.
3. Motorcycle select: large 3D bike, model year, displacement, sourced specs, license-safe image/model.
4. Control choice: manual/automatic, keyboard or touch, sensitivity and assistance.
5. Racing HUD: speed in km/h, RPM, current gear, race time/lap, pause and camera.
6. Pause/settings: resume, restart, graphics, sound and control customization.
7. Results: lap results and restart, with clear distinction between local and verified online results.

## Visual design requirements

Use a coherent dark motorsport palette with restrained accent color for speed/gear/flags, consistent scale and spacing, and strong typographic hierarchy. Confirm the exact colors, fonts and final brand name in Phase 00; none is approved yet.

## Keyboard and mobile flow

Desktop: W/S/A/D or arrow inputs, manual Q/E shifting, shift clutch, rear brake and camera/reset. Mobile: landscape full-canvas racing view; lower-corner touch steer/throttle/brake; optional shifting and clutch controls in manual mode; support simultaneous touches and safe areas. All UI controls must be connected to real game actions.

## Motion rules

Animation must signal physics or state changes: tachometer needles respond to engine RPM, camera feedback corresponds to acceleration/lean, gear indicator transitions upon actual shift completion, warning animations correspond to actual events. Avoid ornamental loading animations with no relationship to readiness.

## Accessibility

Ensure legible contrast, color-independent status cues, scalable UI, reasonable touch targets, reduced motion/camera shake option, distinct audio controls and configurable steering sensitivity. Avoid claiming universal accessibility without testing.

## Acceptance at UX integration gate

At realistic viewing distances on targeted devices, controls do not obscure the track dangerously; critical information is readable; no dead menu actions; touch and desktop workflows complete without keyboard-only traps on mobile.

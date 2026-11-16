# 3D Asset, Sound and Licensing Strategy

**Status:** No assets acquired, approved or integrated.

## Core rule

An asset being downloadable, searchable, generated, or visible on a website does **not** establish that it may be commercially used, adapted or redistributed in a game. Record and verify rights before integration.

## Recommended sequence

- **Prototype:** original low-poly bike and track geometry created in code or Blender; no external IP required.
- **Vertical slice art:** one optimized, riggable motorcycle model with verifiable licensing and stable part separation.
- **Track art:** accurate road surfaces, normal maps, decals, guardrails, vegetation and background props with a consistent style.
- **Audio:** original or appropriately licensed RPM-layered engine samples, surface contact, shifting and collisions.
- **Later catalog:** additional motorcycle variants only when technical data, art quality and licenses meet the same bar.

## Asset inventory schema

Record for each file or bundle:

`asset_id | filename | category | creator | source_url | download_date | license_name | license_url | permitted_commercial_use | redistribution_constraints | attribution_text | model_variant | original_units | triangles | texture_sizes | LOD_levels | reviewer | approval_status`.

Store attribution details where users can access them if required by the license. Keep original proof (license text, purchase receipt or correspondence) in a privately controlled asset records location; do not expose sensitive receipts in a public repository.

## Manufacturers and real motorcycles

Manufacturer names, trademarked logos, official bike design trade dress, marketing images and 3D scans can involve separate rights. Technical data does not grant a 3D model license. Begin with original fictional bikes calibrated using realistic categories if branded rights are unclear.

For published bike prices, store market/region, currency, year, source and last verification date. Do not present a price from another market as the live Bangladesh retail price. Do not claim 2026 MSRP is permanent.

## Geometry and animation quality gates

- Units match meters; wheel radius and wheelbase align with configuration data.
- Pivots exist for wheel spin, front steering, suspension movement and rider animation as required.
- Physics collider shapes do not require detailed mesh triangles.
- Textures and meshes include mobile-friendly LOD and compressed alternatives when appropriate.
- Models show correct tire contact and do not detach visually during steering/lean.
- A representative track scene and bike are profiled under real hardware constraints.

## Third-party skills and repositories

Skill descriptions are not proof of code quality or safety. Review any downloaded SKILL.md, script, hooks, license and network permissions before adoption. External skills are **not installed** as part of this planning pack.

## Acquisition register

| Asset | Status | Source | Approval |
| --- | --- | --- | --- |
| First playable bike proxy | Not created | Original geometry planned | Pending |
| First high-quality bike model | Not selected | TBD | Pending |
| First circuit assets | Not selected | TBD | Pending |
| Engine audio set | Not selected | TBD | Pending |
| Environmental SFX | Not selected | TBD | Pending |

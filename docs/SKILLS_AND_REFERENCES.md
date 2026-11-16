# Skills, Official Documentation and External Dependencies

**No third-party skill has been installed by this planning package.** The two `motorcycle-physics` skill folders included here are locally authored instruction files, not external packages or proof of verified physics.

**Phase 00 audit (2026-11-17):** see [`ENVIRONMENT_AUDIT.md` §5](ENVIRONMENT_AUDIT.md) for verified install status. In short: the bufatechno repository was found cloned into `.agents/skills/` (not loaded by Claude Code; recommended for removal); `frontend-design` is installed at user level; OpenAI `game-playtest` is not installed.

## How skills are handled

- Codex uses project skills in `.agents/skills/<name>/SKILL.md` where supported.
- Claude Code uses project skills in `.claude/skills/<name>/SKILL.md`.
- The custom motorcycle skill has matching provider-specific copies. Update both together and keep detailed engineering content canonical in `docs/PHYSICS_AND_CALIBRATION.md`.
- Review community skill sources and bundled scripts before installing. Pin versions where possible, review permissions, and record exactly which versions were used.
- Use engine-independent design and playtest skill guidance when compatible. Do not use Three.js-specific code in the Babylon.js runtime.

## Candidate skills: evaluate at Phase 00, not assumed installed

- Community web game skill: https://github.com/bufatechno/bufatechno-webgamedev
- Anthropic frontend-design: https://github.com/anthropics/skills/tree/main/skills/frontend-design
- OpenAI Game Studio: https://github.com/openai/plugins/tree/main/plugins/game-studio (confirm availability in installed environment)
- Three.js skills: https://github.com/majidmanzarpour/threejs-game-skills (not primary for a Babylon project)

## Verified official starting points (accessed 17 Nov 2026)

- Babylon physics and V2: https://doc.babylonjs.com/features/featuresDeepDive/physics
- Babylon Havok integration: https://doc.babylonjs.com/features/featuresDeepDive/physics/havokPlugin/
- MDN WebGPU support/secure context: https://developer.mozilla.org/en-US/docs/Web/API/GPU
- MDN Web Audio autoplay restrictions: https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay
- Claude Code CLAUDE.md guidance: https://support.claude.com/en/articles/14553240-give-claude-context-claude-md-and-better-prompts
- Claude Code project skills: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- OpenAI Codex AGENTS.md workflow: https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex
- OpenAI skills guidance: https://developers.openai.com/api/docs/guides/tools-skills

This links list is a starting bibliography, not a guarantee that all referenced third-party installations or APIs work with a particular local environment. Verify versions, compatibility and license when Phase 00 is authorized.

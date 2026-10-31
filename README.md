# Backflip

A portable agent skill for building and reviewing Flipper Zero applications. It emphasizes pixel-safe 128×64 layouts, actual viewport dimensions, navigation, FAP SDK compatibility, resource ownership and verification.

## Install

Copy **this entire folder** as `backflip` into the relevant skills directory:

| Agent | Project scope | User scope |
|---|---|---|
| Antigravity | `.agents/skills/` | `~/.gemini/config/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex | `.agents/skills/` | `~/.agents/skills/` |
| OpenCode | `.opencode/skills/` | `~/.config/opencode/skills/` |

For example, a Claude Code project ends up with `.claude/skills/backflip/SKILL.md` and the adjacent `references/` directory. Antigravity, Codex and OpenCode also document `.agents/skills/` project discovery; Claude Code's documented project path differs. OpenCode supports additional compatibility paths. `~` means the agent process's home, including inside WSL. Consult [portability research](research/portability.md) for sources and caveats. Restart or refresh the agent if required, and verify it can discover the skill. Discovery and invocation were not runtime-tested across these products.

Ask the agent to use the `backflip` skill when designing, implementing or reviewing a FAP. The skill's metadata also describes automatic trigger conditions, but discovery and auto-invocation depend on the product and its settings.

## Contents

- [SKILL.md](SKILL.md): compact workflow and triggers.
- [Pixel-safe layout](references/layout.md): coordinate spaces, bounds, collisions, text and verification.
- [FAP engineering](references/engineering.md): APIs, navigation, ownership, callbacks and testing.
- [Sources](references/sources.md): evidence and links to the detailed research.

## License and attribution

Original documentation in this repository is licensed under [Creative Commons Attribution 4.0 International](LICENSE) (CC BY 4.0). Attribute “Backflip contributors”, link to the license and indicate modifications. Source projects and trademarks remain with their respective owners, under their own licenses; citations do not relicense upstream code. This skill is documentation, not a firmware SDK or a license grant to third-party code. Creative Commons recommends software-specific licenses for software; select one separately before adding runnable code.

# Agent skill portability and licensing research

Research date: 2026-10-31. This is a **documentation review**, not a runtime installation or invocation test. Paths below are documented defaults; configuration, permissions, product versions, duplicate names, or disabled discovery can alter behavior. `~` and `$HOME` refer to the user's home directory in the environment where that agent runs (including WSL, if applicable).

## Portable directory and manifest

The [Agent Skills specification](https://agentskills.io/specification) describes one directory per skill, with mandatory `SKILL.md`: YAML frontmatter followed by Markdown instructions. `name` and `description` are required. For a conservative intersection with [OpenCode's restrictions](https://opencode.ai/docs/skills/#validate-names), use a 1–64 character lowercase ASCII alphanumeric name with single internal hyphens, exactly matching the directory name. Use a nonempty, task-specific description of at most 1,024 characters. Optional `references/`, `scripts/`, and `assets/` may contain supplementary material. Link supporting files relative to `SKILL.md`; a shared format does not guarantee identical tool availability, commands, operating systems, invocation, or execution.

```text
backflip/
├── SKILL.md                 # name: backflip; description: …
└── references/              # optional detailed material
```

Discovery generally advertises metadata first and loads instructions when used, but actual triggering is product-dependent. Do not claim that matching a description guarantees invocation.

## Documented installation locations

Copy the **entire** `backflip/` folder to one destination appropriate to the product; append `backflip/SKILL.md` to each path below.

| Agent | Project directory | User directory | Evidence and caveats |
|---|---|---|---|
| Antigravity | `<workspace-root>/.agents/skills/` | `~/.gemini/config/skills/` | [Official skills documentation](https://antigravity.google/docs/skills/) documents both paths and parent-folder discovery up to repository root. `.agent/skills/` is backward compatible. The App also supports legacy `~/.gemini/antigravity/skills/`; CLI has additional paths not read by the App. Use the common current global path, not a CLI-only path. |
| Claude Code | `<project-root>/.claude/skills/` | `~/.claude/skills/` | [Official skills documentation](https://code.claude.com/docs/en/skills) documents project and personal scopes. Nested `.claude/skills/` paths are discovered when working in relevant subdirectories. The reviewed docs do not establish `.agents/skills/` as a Claude Code discovery path. |
| Codex | `<repo-root>/.agents/skills/` | `$HOME/.agents/skills/` | [Official Codex skills guide](https://developers.openai.com/codex/skills) documents repository traversal from CWD to root and the user path. It also lists `/etc/codex/skills` for administration and bundled system skills. If a change does not appear, restart Codex. |
| OpenCode | `<project-root>/.opencode/skills/` | `~/.config/opencode/skills/` | [Official locations](https://opencode.ai/docs/skills/#place-files) also document project `.claude/skills/` and `.agents/skills/`, and global `~/.claude/skills/` and `~/.agents/skills/` compatibility locations. [Discovery](https://opencode.ai/docs/skills/#understand-discovery) walks from CWD to Git worktree. Skill permissions can restrict availability. |

**Consequence:** `.agents/skills/backflip/` is a documented shared project path for Antigravity, Codex, and OpenCode. Claude Code requires its documented `.claude/skills/` location; do not promise one physical directory works automatically for all four. A copy in each required location is portable; symlink support has not been established here. OpenCode can discover Claude's project copy too, so avoid duplicate same-name installations when combining locations unless product-specific precedence has been verified.

## License and attribution

Use **Creative Commons Attribution 4.0 International (CC BY 4.0)** for original prose/documentation if the copyright holder approves. The [canonical complete English legal code](https://creativecommons.org/licenses/by/4.0/legalcode.en) is available as official [plain text](https://creativecommons.org/licenses/by/4.0/legalcode.txt), suitable for including in a `LICENSE` file without alteration. The [human-readable deed](https://creativecommons.org/licenses/by/4.0/) is a summary, not a substitute for the legal code. This research file links to the full canonical text; the deliverable should bundle the full official text.

CC BY 4.0 permits sharing and adaptation, including commercial use, subject to its conditions. Under [Section 3(a)](https://creativecommons.org/licenses/by/4.0/legalcode.en#s3a), when sharing material retain creator/attribution party, copyright notice, license notice, warranty disclaimer notice, and source URI when supplied and reasonably practicable; indicate modifications and preserve earlier modification indications; identify and link or provide the license. Additional legal or technological restrictions cannot prevent recipients from exercising licensed rights. The actual legal code governs; this is not legal advice.

A practical notice for the original prose, with actual rights-holder identity and year supplied by the publisher:

> © YEAR RIGHTS HOLDER. Original prose licensed under Creative Commons Attribution 4.0 International (https://creativecommons.org/licenses/by/4.0/). Source: PROJECT URL.

Do not invent a rights holder or project URL. Mark adapted source material, credit its authors, link its source and license where required, and respect each source's own license; CC BY cannot relicense third-party copyrighted material without permission. Facts, API identifiers, links, and independently written summaries are different from copied source code or text; review quotations and examples individually. The [Creative Commons FAQ](https://creativecommons.org/faq/#can-i-apply-a-creative-commons-license-to-software) recommends software-specific licenses for software rather than CC licenses. If executable scripts or substantial code examples are added, confirm their ownership and select an appropriate software license separately; no new code license is implied here.

## Verification

- Primary sources reviewed: Agent Skills specification, each vendor's skills documentation, CC BY 4.0 legal code and official text, and CC FAQ.
- Build, type-check, lint, automated tests, coverage, and runtime discovery: **N/A / not run** for this research-only ticket. No agent runtime compatibility claimed.
- Review remains necessary for source code/example licensing and any actual installation on target product versions.

# Claude Code Skills

**Type:** SOP
**Category:** Operations
**Date:** 2026-06-25
**Related:** [[claude-code-video-pipeline]]

---

## What they are

Skills are slash commands (`/skill-name`) that extend Claude Code with pre-loaded instructions and personas. They only work in **Claude Code CLI and Desktop App** — not in Claude.ai chat or the Web UI.

## Source of truth

Skills are stored in **SingleBrain** (`skills/` folder) and synced to each machine manually.

- **Repo:** https://github.com/cleverwolfdigital/SingleBrain
- **Local path:** `~/.claude/skills/`

## Install on a new machine

```bash
git clone https://github.com/cleverwolfdigital/SingleBrain.git
cp -R SingleBrain/skills/* ~/.claude/skills/
```

## Installed skills

| Skill | Invoke | Description |
|---|---|---|
| `frontend-design` | `/frontend-design` | Opinionated UI design lead — distinctive palettes, typography, layout. Installed 2026-06-25 from `anthropics/skills` repo |

## Adding a new skill

```bash
# Install locally first
cp -R ~/.claude/skills/<skill-name> ~/SingleBrain/skills/
cd ~/SingleBrain && git add skills/ && git commit -m "add <skill-name> skill" && git push
```

## Where to find more skills

- Official Anthropic skills: https://github.com/anthropics/skills
- Available skills in that repo: algorithmic-art, brand-guidelines, canvas-design, claude-api, doc-coauthoring, docx, frontend-design, internal-comms, mcp-builder, pdf, pptx, skill-creator, slack-gif-creator, theme-factory, web-artifacts-builder, webapp-testing, xlsx
